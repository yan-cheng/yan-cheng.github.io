
在开发中，会遇到有一些本地JAR包在Maven的公共仓库中没有，所以我们要在本地引入。

## 方法 1
使用指令将本地JAR包安装到你的本地Maven Repository中。官方安装介绍在这里：
http://maven.apache.org/guides/mini/guide-3rd-party-jars-local.html

安装 JAR包到本地Maven Repository中的指令为：
```
Install the JAR into your local Maven repository as follows:

mvn install:install-file
   -Dfile=<path-to-file>
   -DgroupId=<group-id>
   -DartifactId=<artifact-id>
   -Dversion=<version>
   -Dpackaging=<packaging>
   -DgeneratePom=true

Where: <path-to-file>  the path to the file to load
       <group-id>      the group that the file should be registered under
       <artifact-id>   the artifact name for the file
       <version>       the version of the file
       <packaging>     the packaging of the file e.g. jar
```

比如，现在将 安装到本地Maven Repository中：

```bash
mvn install:install-file \
    -Dfile=/localpath/servicebinder-0.9.0-SNAPSHOT.jar \
    -DgroupId=org.apache.felix \
    -DartifactId=servicebinder \
    -Dversion=0.9.0-SNAPSHOT \
    -Dpackaging=jar
```

## 方法 2
直接在 pom.xml 中添加本地依赖关系。比如，下面添加了本地Jar文件：ProjectDir/src/main/resources/sample.jar。

```xml
<dependency>
    <groupId>com.sample</groupId>
    <artifactId>sample</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/src/main/resources/yourJar.jar</systemPath>
</dependency>
```
