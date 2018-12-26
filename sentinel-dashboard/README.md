# Sentinel 控制台

## 0. 概述

Sentinel 控制台是流量控制、熔断降级规则统一配置和管理的入口，它为用户提供了机器自发现、簇点链路自发现、监控、规则配置等功能。在 Sentinel 控制台上，我们可以配置规则并实时查看流量控制效果。

## 1. 编译和启动

### 1.1 如何编译

使用如下命令将代码打包成一个 fat jar:

```bash
mvn clean package
```

### 1.2 如何启动

使用如下命令启动编译后的控制台：

```bash
java -Dserver.port=8080 \
-Dcsp.sentinel.dashboard.server=localhost:8080 \
-Dproject.name=sentinel-dashboard \
-jar target/sentinel-dashboard.jar
```

上述命令中我们指定几个 JVM 参数，其中 `-Dserver.port=8080` 用于指定 Spring Boot 启动端口为 `8080`，其余几个是 Sentinel 客户端的参数。
为便于演示，我们对控制台本身加入了流量控制功能，具体做法是引入 `CommonFilter` 这个 Sentinel 拦截器。上述 JVM 参数的含义是：

| 参数 | 作用 |
|--------|--------|
|`Dcsp.sentinel.dashboard.server=localhost:8080`|向 Sentinel 客户端指定控制台的地址|
|`-Dproject.name=sentinel-dashboard`|向 Sentinel 指定本程序名称|

全部配置项参考 [启动配置项](https://github.com/alibaba/Sentinel/wiki/%E5%90%AF%E5%8A%A8%E9%85%8D%E7%BD%AE%E9%A1%B9)

经过上述配置，控制台启动后会自动向自己发送心跳。程序启动后浏览器访问`localhost:8080`即可访问 Sentinel 控制台。

## 2. 客户端接入
```xml
<dependency>
			<groupId>com.alibaba.csp</groupId>
			<artifactId>sentinel-core</artifactId>
			<version>1.4.0</version>
		</dependency>
		<dependency>
			<groupId>com.alibaba.csp</groupId>
			<artifactId>sentinel-dubbo-adapter</artifactId>
			<version>1.4.0</version>
		</dependency>
		<dependency>
			<groupId>com.alibaba.csp</groupId>
			<artifactId>sentinel-transport-simple-http</artifactId>
			<version>1.4.0</version>
		</dependency>
		<dependency>
			<groupId>com.alibaba.csp</groupId>
			<artifactId>sentinel-cluster-client-default</artifactId>
			<version>1.4.0</version>
		</dependency>
		<dependency>
			<groupId>com.alibaba.csp</groupId>
			<artifactId>sentinel-cluster-server-default</artifactId>
			<version>1.4.0</version>
		</dependency>
		<dependency>
			<groupId>com.alibaba.csp</groupId>
			<artifactId>sentinel-datasource-zookeeper</artifactId>
			<version>1.4.0</version>
		</dependency>

```

## 3. 验证是否接入成功

客户端正确配置并启动后，会主动向控制台发送心跳包，汇报自己的存在；控制台收到客户端心跳包之后，会在左侧导航栏中显示该客户端信息。控制台能够看到客户端的机器信息，则表明客户端接入成功了。


更多：[控制台功能介绍](./Sentinel_Dashboard_Feature.md)。
