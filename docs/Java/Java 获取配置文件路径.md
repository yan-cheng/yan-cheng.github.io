在Java中，获取资源文件路径
首先，Java中的getResourceAsStream有以下几种：
1. Class.getResourceAsStream(String path) ： path 不以’/'开头时默认是从此类所在的包下取资源，以’/'开头则是从ClassPath根下获取。其只是通过path构造一个绝对路径，最终还是由ClassLoader获取资源。
2. Class.getClassLoader.getResourceAsStream(String path) ：默认则是从ClassPath根下获取，path不能以’/'开头，最终是由ClassLoader获取资源。  
3. ServletContext. getResourceAsStream(String path)：默认从WebAPP根目录下取资源，Tomcat下path是否以’/'开头无所谓，当然这和具体的容器实现有关。

举个例子，比如下面这个目录树就是一个maven项目编译之后的文件分布情况：  
```
.  
├── auth.server.properties  
├── com  
│ ├── winwill  
│ │ └── test  
│ │ ├── TestConstants.class  
│ │ ├── TestConstants$HOST.class  
│ │ ├── TestGetResourceAsStream.class  
│ │ └── TestGuava.class  
│ │ └── config1.properties  
│ └── xiaomi  
│ │ └── config2.properties  
│ └── xmpush  
│ └── ios  
│ ├── IOSPushTest.class  
│ ├── UploadCertificate$1.class  
│ └── UploadCertificate.class  
├── config  
│ └── config3.properties  
├── config4.properties  
├── kafka-consumer.properties  
├── kafka-producer.properties  
├── META-INF  
│ ├── MANIFEST.MF  
│ └── maven  
│ └── com.winwill.test  
│ └── test  
│ ├── pom.properties  
│ └── pom.xml  
├── redis.properties  
└── zookeeper.properties
```
在TestGetResourceAsStream.class这个类中获取 com.winwill.config1.properties，com.xiaomi.config2.properties，config.config3.properties，config4.properties 这四个位置的配置文件：

```java
// 获取config1.properties
InputStream config1 = this.getClass().getResourceAsStream("config1.properties");

// 获取config2.properties
InputStream config2 = this.getClass().getResourceAsStream("/com/xiaomi/config2.properties");
InputStream config2_another = this.getClass().getResourceAsStream("../xiaomi/config2.properties");

// 获取config3.properties
InputStream config2 = this.getClass().getResourceAsStream("/config/config3.properties");

// 获取config4.properties
InputStream config2 = this.getClass().getResourceAsStream("/config4.properties");
```
注意区别 `Class.getResourceAsStream(String path)` 与 `Class.getClassLoader().getResourceAsStream(String path)` 的区别。
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUyMTYxNjYsMTE5OTU4ODkyMF19
-->