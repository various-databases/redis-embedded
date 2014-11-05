嵌入式Redis服务
==============

> 嵌入式Redis服务器，用于集成测试。


pom中依赖配置
==============

仓库地址:
```
	<repositories>
		<!-- 中新仓库 -->
		<repository>
			<id>zxsoft-public</id>
			<name>Nexus Release Repository</name>
			<url>http://192.168.3.23:18081/nexus/content/groups/public/</url>
		</repository>
	</repositories>
```

依赖包:
```
<dependency>
   <groupId>zx.soft</groupId>
   <artifactId>redis-embedded</artifactId>
   <version>1.0.0</version>
</dependency>
```
具体见：https://github.com/Various-DBs/redis-embedded

使用示例
==============

运行RedisServer如下:
```java
RedisServer redisServer = new RedisServer(6379);
redisServer.start();
// do some work
redisServer.stop();
```
使用Redis目录运行RedisServer:
```java
RedisServer redisServer = new RedisServer("/path/to/your/redis", 6379);
```
使用流式接口运行RedisServer:
```java
RedisServer redisServer = RedisServer.builder()
  .executable("/path/to/your/redis")
  .port(6379)
  .slaveOf("locahost", 6378)
  .configFile("/path/to/your/redis.conf")
  .build();
```
或者创建redis.conf配置:
```java
RedisServer redisServer = RedisServer.builder()
  .executable("/path/to/your/redis")
  .port(6379)
  .slaveOf("locahost", 6378)
  .setting("daemonize no")
  .setting("appendonly no")
  .build();
```
Redis集成测试示例如下:
```java
public class SomeIntegrationTestThatRequiresRedis {
  private RedisServer redisServer;
  
  @Before
  public void setup() throws Exception {
    redisServer = new RedisServer(6379); // or new RedisServer("/path/to/your/redis", 6379);
    redisServer.start();
  }
  
  @Test
  public void test() throws Exception {
    // testing code that requires redis running
  }
  
  @After
  public void tearDown() throws Exception {
    redisServer.stop();
  }
}
```

Redis版本
==============

当前的版本是:
- Linux/Unix：Redis 2.6.14
- Windows：非官方的Win32/64版本可从https://github.com/MSOpenTech/redis (分支 2.6)上下载

注意，在使用的时候需要指定版本。
