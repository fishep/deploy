# jdk 安装
> 安装各个版本的 jdk

#### 部署
```shell

# 先安装 jetbrains 的 idea
# File -> Project Structure -> SDK: -> Add JDK 
# 选择版本，选择厂商，最好选择 Eclipse Temurin

# 配置环境变量JAVA_HOME  ~/usr/local/openjdk/temurin-17.0.15
# 配置环境变量JAVA21_HOME ~/usr/local/openjdk/temurin-21.0.7

# 将环境变量的bin目录，加入PATH

java --version

```

#### 安装 maven
```shell
#将 ~/usr/bin/ 设置成环境变量，然后加入PATH

#找到idea自带的maven，/Applications/IntelliJ IDEA.app/Contents/plugins/maven/lib/maven3/bin/mvn

cd ~/usr/bin/
ln -s /Applications/IntelliJ IDEA.app/Contents/plugins/maven/lib/maven3/bin/mvn mvn

mvn --version
```

#### 安装 gradle
```shell
#将 ~/usr/bin/ 设置成环境变量，然后加入PATH

#找到idea自动下载的gradle，  ~/.gradle/wrapper/dists/gradle-8.14.2-bin/2pb3mgt1p815evrl3weanttgr/gradle-8.14.2/bin/gradle

cd ~/usr/bin/
ln -s ~/.gradle/wrapper/dists/gradle-8.14.2-bin/2pb3mgt1p815evrl3weanttgr/gradle-8.14.2/bin/gradle gradle

gradle --version

```