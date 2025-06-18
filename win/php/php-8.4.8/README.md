# deploy php-8.4.8
> 手动安装预编译的二进制文件

#### 安装
```shell

#下载地址
#https://windows.php.net/downloads/releases/php-8.4.8-nts-Win32-vs17-x64.zip


#解压到目录
#E:\usr\local\php\php-8.4.8

```

#### 验证php安装成功
```shell
#配置环境变量
#cmd
#set PHP_HOME=E:\usr\local\php\php-8.4.8
#set Path=%Path%;%PHP_HOME%
#powershell
$env:PHP_HOME = "E:\usr\local\php\php-8.4.8"
$env:Path += ";$env:PHP_HOME"

php -v

php phpinfo.php

php -S 0.0.0.0:8000
# 访问页面 http://localhost:8000/phpinfo.php


vim php.ini
cgi.force_redirect = 0
fastcgi.impersonate = 1

vim php.ini
extension_dir = "ext"

php-cgi --help
php-cgi -b 0.0.0.0:9000

```

#### 安装扩展
```shell
# 查看PHP编译信息
php -i

# 官方扩展库
#https://windows.php.net/downloads/pecl/releases/

# 下载扩展库
#php_redis-x.x.x-8.4-nts-vc15-x64.zip
#php_xdebug-x.x.x-8.4-nts-vc15-x64.zip

#解压后将扩展库复制到到PHP的ext目录
#php_redis.dll
#php_xdebug.dll

#添加配置
echo "extension=php_redis.dll" >> php.ini
echo """
zend_extension=php_xdebug.dll
xdebug.mode=develop,debug

#xdebug.start_with_request=yes
xdebug.start_with_request=trigger
xdebug.trigger_value=PHPSTORM
xdebug.log_level=0

xdebug.var_display_max_children=10
xdebug.var_display_max_data=10
xdebug.var_display_max_depth=10

xdebug.discover_client_host=true
xdebug.client_host=win.local
xdebug.client_port=9003
""" >> php.ini

php -m | findstr redis
php -m | findstr xdebug

```

#### 安装 composer
```shell
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php --install-dir="$env:PHP_HOME" --version=2.8.1
#php -r "unlink('composer-setup.php');"

#新建文件添加内容
composer.bat
@php "%~dp0composer.phar" %*

composer -V
composer --help
composer config repo.packagist composer https://mirrors.aliyun.com/composer/
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

```

#### 触发 XDEBUG
```shell
# web
?XDEBUG_TRIGGER=PHPSTORM
#或
?XDEBUG_SESSION=PHPSTORM

# cli
export XDEBUG_TRIGGER=PHPSTORM
#或
export XDEBUG_SESSION=PHPSTORM
php myscript.php

```