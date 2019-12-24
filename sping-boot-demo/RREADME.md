* 指定环境（application.yml）

  ```yml
  server:
    port: 8080
    servlet:
      context-path: /demo
  spring:
    profiles:
      active: dev
  
  ```

* 环境配置（application-dev.yml）

  ```yml
  application:
    name: dev环境 @artifactId@
    version: dev环境 @version@
  developer:
    name: dev环境 xkcoding
    website: dev环境 http://xkcoding.com
    qq: dev环境 237497819
    phone-number: dev环境 17326075631
  ```

* 读取环境配置

  * 第一种方式

    ```java
    @Data
    @ConfigurationProperties(prefix = "developer")
    @Component
    public class DeveloperProperty {
    	private String name;
    	private String website;
    	private String qq;
    	private String phoneNumber;
    }
    ```

  * 第二种方式

    ```java
    @Data
    @Component
    public class ApplicationProperty {
    	@Value("${application.name}")
    	private String name;
    	@Value("${application.version}")
    	private String version;
    }
    ```




#### 注解

* @JsonProperty

  此注解用于属性上，作用是把该属性的名称序列化为另外一个名称，如把trueName属性序列化为name，@JsonProperty("name")。 

