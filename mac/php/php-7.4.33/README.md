# deploy php-7.4.33
> 源码编译安装

#### 下载源码
```shell
git clone git@github.com:php/php-src.git
cd php-src
git checkout -b php-7.4.33 php-7.4.33
```

#### 要求
```shell
make -v
gcc -v
autoconf -V # PHP 7.3 and later: 2.68+ 
bison -V # PHP 7.4 and later: 3.0.0+ 
re2c -v # PHP 8.3 and later: 1.0.3+ 

```

#### 安装依赖库
```shell
# 系统已经存在的库，但版本不符合要求
brew info bison
brew install bison
$(brew --prefix bison)/bin/bison -V

brew info re2c
brew install re2c
re2c -v

# 检出旧版本
brew tap homebrew/core --force
brew tap-new local/tap
brew extract icu4c --version=67.1 local/tap

# 特定模块依赖的组件
brew install libiconv
brew install oniguruma # ./configure --enable-mbstring
brew install jpeg libpng webp freetype2 # ./configure --enable-gd --with-jpeg --with-freetype --with-webp
brew install expat libxml2 # ./configure --enable-xml --enable-simplexml --enable-dom --with-libxml
brew install icu4c@67.1 # ./configure --enable-intl
brew install libxml2 curl # ./configure --enable-soap 
brew install curl # ./configure --with-curl
brew install openssl@1.1 # ./configure --with-openssl
brew install zlib libzip bzip2 # ./configure --with-zlib --with-zip --with-bz2
brew install gettext # ./configure --with-gettext


# 使用自定义的bison
export PATH=$(brew --prefix bison)/bin:$PATH
# 配置icu被发现  echo $(brew --prefix icu4c) ls $(brew --prefix icu4c)
# 配置icu被发现  echo $(brew --prefix icu4c@67.1) ls $(brew --prefix icu4c@67.1)
#export PKG_CONFIG_PATH=$(brew --prefix icu4c@67.1)"/lib/pkgconfig"
#pkg-config --modversion icu-i18n
#pkg-config --modversion openssl

#如果有多个版本，确保使用的版本正确
brew unlink openssl
brew link openssl@1.1 --overwrite --force
pkg-config --modversion openssl
pkg-config --cflags --libs openssl

pkg-config --modversion icu-i18n
```

#### 安装
```shell
export PHP_HOME=~/usr/local/php/php-7.4.33

./buildconf --force

./configure --help

./configure \
  --prefix=$PHP_HOME \
  --with-config-file-path=$PHP_HOME/etc \
  --enable-cli \
  --enable-fpm \
  --with-fpm-user=_www \
  --with-fpm-group=_www \
  --disable-debug \
  --disable-phpdbg \
  --enable-sockets \
  --enable-pcntl \
  --enable-bcmath \
  --enable-mbstring \
  --enable-gd --with-jpeg=$(brew --prefix jpeg) --with-freetype=$(brew --prefix freetype) --with-webp=$(brew --prefix webp) \
  --enable-xml --enable-simplexml --enable-dom --with-libxml=$(brew --prefix libxml2) \
  --enable-intl \
  --enable-soap \
  --without-pcre-jit \
  --with-mysqli=mysqlnd \
  --with-pdo-mysql=mysqlnd \
  --with-curl=$(brew --prefix curl) \
  --with-openssl=$(brew --prefix openssl@1.1) \
  --with-iconv=$(brew --prefix libiconv) \
  --with-zlib=$(brew --prefix zlib) \
  --with-zip=$(brew --prefix libzip) \
  --with-bz2=$(brew --prefix bzip2) \
  --with-gettext=$(brew --prefix gettext)

make -j$(sysctl -n hw.logicalcpu)
#make test
make install

cp php.ini-development $PHP_HOME/etc/php.ini
cp $PHP_HOME/etc/php-fpm.conf.default $PHP_HOME/etc/php-fpm.conf
cp $PHP_HOME/etc/php-fpm.d/www.conf.default $PHP_HOME/etc/php-fpm.d/www.conf

```

#### 验证php安装成功
```shell
export PATH=$PHP_HOME/bin:$PHP_HOME/sbin:$PATH

php -v

php phpinfo.php

php -S 0.0.0.0:8000
# 访问页面 http://localhost:8000/phpinfo.php


php-fpm --help
sudo php-fpm -F

ps aux | grep php-fpm
ps aux -o ppid | grep php-fpm
pgrep php-fpm | xargs -I {} ps -o pid,ppid,comm -p {}
sudo kill -TERM 86768 # 优雅关闭
pkill php-fpm
```

#### 使用phpize安装扩展
```shell
phpize -v

# 安装 php 源码里的扩展
ls php-src/ext
cd php-src/ext/ftp
phpize 
./configure --with-php-config=$PHP_HOME/bin/php-config
make
make install
vim php.ini # 开启ftp扩展
php -m | grep ftp

# 安装第三方扩展
wget https://pecl.php.net/get/redis-5.3.7.tgz
tar -zxvf redis-5.3.7.tgz
cd redis-5.3.7

phpize 
./configure --with-php-config=$PHP_HOME/bin/php-config
make
make install

echo >> php.ini # 换行
echo extension=redis.so >> php.ini

php -m | grep redis
```

#### 安装 pear
```shell
安装到 $PHP_HOME/pear

curl -O https://pear.php.net/go-pear.phar
php -d detect_unicode=0 go-pear.phar

./pear/bin/pear version
./pear/bin/pecl version

./pear/bin/pecl install xdebug-3.1.6

echo """
zend_extension=xdebug
xdebug.mode=develop,debug

xdebug.start_with_request=yes
#xdebug.start_with_request=trigger
xdebug.trigger_value=PHPSTORM
xdebug.log_level=0

xdebug.var_display_max_children=10
xdebug.var_display_max_data=10
xdebug.var_display_max_depth=10

xdebug.discover_client_host=true
xdebug.client_host=mac.local
xdebug.client_port=9003
xdebug.idekey=PHPSTORM
""" >> php.ini

php -m | grep xdebug

```

#### 安装 composer
```shell
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
#php composer-setup.php --install-dir=$PHP_HOME/bin/ --filename=composer --version=1.8.6
php composer-setup.php --install-dir=$PHP_HOME/bin/ --filename=composer --version=2.8.1
#php -r "unlink('composer-setup.php');"

composer -V
composer --help
composer config repo.packagist composer https://mirrors.aliyun.com/composer/
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

```
