# 一、服务端三层框架

## 1.表现层(SpringMVC)

### 1.1.MVC设计模型

* 结构

  > M:model,模型层
  >
  > V:view,视图层
  >
  > C:controll,控制层
  
* 配置步骤
  * 依赖导入
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
    
      <groupId>com.zjs</groupId>
      <artifactId>day8_6</artifactId>
      <version>1.0-SNAPSHOT</version>
      <packaging>war</packaging>
    
      <name>day8_6 Maven Webapp</name>
      <!-- FIXME change it to the project's website -->
      <url>http://www.example.com</url>
    
      <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.7</maven.compiler.source>
        <maven.compiler.target>1.7</maven.compiler.target>
        <!-- Spring版本锁定 -->
        <spring.version>5.0.2.RELEASE</spring.version>
      </properties>
    
      <dependencies>
        <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.11</version>
          <scope>test</scope>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>${spring.version}</version>
        </dependency>
    
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-web</artifactId>
          <version>${spring.version}</version>
        </dependency>
    
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>${spring.version}</version>
        </dependency>
    
        <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>servlet-api</artifactId>
          <version>2.5</version>
          <scope>provided</scope>
        </dependency>
    
        <dependency>
          <groupId>javax.servlet.jsp</groupId>
          <artifactId>jsp-api</artifactId>
          <version>2.0</version>
          <scope>provided</scope>
        </dependency>
      </dependencies>
    
      <build>
        <finalName>day8_6</finalName>
        <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
          <plugins>
            <plugin>
              <artifactId>maven-clean-plugin</artifactId>
              <version>3.1.0</version>
            </plugin>
            <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
            <plugin>
              <artifactId>maven-resources-plugin</artifactId>
              <version>3.0.2</version>
            </plugin>
            <plugin>
              <artifactId>maven-compiler-plugin</artifactId>
              <version>3.8.0</version>
            </plugin>
            <plugin>
              <artifactId>maven-surefire-plugin</artifactId>
              <version>2.22.1</version>
            </plugin>
            <plugin>
              <artifactId>maven-war-plugin</artifactId>
              <version>3.2.2</version>
            </plugin>
            <plugin>
              <artifactId>maven-install-plugin</artifactId>
              <version>2.5.2</version>
            </plugin>
            <plugin>
              <artifactId>maven-deploy-plugin</artifactId>
              <version>2.8.2</version>
            </plugin>
          </plugins>
        </pluginManagement>
      </build>
    </project>
    
    ```
  * spring配置
    ```xml
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xmlns:mvc="http://www.springframework.org/schema/mvc"
           xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
    		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
    		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">
         <!--开启注解扫描-->
        <context:component-scan base-package="com.zjs"></context:component-scan>
        <!--视图解析器对象-->
        <bean id="internalResouceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <!--配置文件所在目录-->
            <property name="prefix" value="/WEB-INF/pages/"></property>
            <!--配置后缀名-->
            <property name="suffix" value=".jsp"></property>
        </bean>
        <!--开启SpringMV框架注解的支持:开启这个还默认配置了适配器、映射器-->
        <mvc:annotation-driven></mvc:annotation-driven>
    
    </beans>
    ```
  * web.xml配置
    ```xml
    <!DOCTYPE web-app PUBLIC
     "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
     "http://java.sun.com/dtd/web-app_2_3.dtd" >
    
    <web-app>
      <!--SpringMVC的核心控制器-->
      <display-name>Archetype Created Web Application</display-name>
      <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--参数初始化配置-->
        <init-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>classpath:SpringMvc.xml</param-value>
        </init-param>
        <!--加载次数-->
        <load-on-startup>1</load-on-startup>
      </servlet>
      <servlet-mapping>
        <!-- 在servlet映射中添加dispatcherServlet -->
        <servlet-name>dispatcherServlet</servlet-name>
        <!-- 整个网站全部的Servlet都要经过核心控制器 -->
        <url-pattern>/</url-pattern>
      </servlet-mapping>
    </web-app>
    
    ```
  
* 注解
  * @RequestMapping(path="")
    ```java
    /**
    *作为路径映射。可以放方法或者类上
    *属性：method请求方法
          paramg规定传递参数
    */
    package com.zjs.controller;
    
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;
    
    @Controller
    @RequestMapping(path = "/user")
    public class HelloController {
    
        @RequestMapping(value = "/hello")
        public  String sayHello()
        {
            System.out.println("hello StringMVC");
            return "success";
        }
    }
    
    ```
  
* 参数绑定
  
  * 通过接收参数名和请求当中传递参数名相同可以获得
  
  * 引用类型的封装
  
    ```jsp
    <%@page contentType="text/html; charset=UTF-8" %>
    <html>
    <body>
    <h2>Hello World!</h2>
    <form action="/user/hello" method="post">
        名称:<input type="text" name="name"/>
        金额：<input type="text" name="money"/>
        <input type="submit" value="提交"/>
    </form>
    </body>
    </html>
    
    <!--map或者list类型-->
  <%@page contentType="text/html; charset=UTF-8" %>
    <html>
  <body>
    <h2>Hello World!</h2>
    <form action="/user/hello" method="post">
        名称:<input type="text" name="name"/>
        金额：<input type="text" name="money"/>
    
        用户名称：<input type="text" name="user.username"/>
        用户年龄：<input type="text" name="user.age"/>
    
        账号1用户名称:<input type="text" name="list[0].username"/>
        账号1用户年龄:<input type="text" name="list[0].age"/>
        账号2用户名称：<input type="text" name="map[one].username">
        账号2用户年龄:<input type="text" name="map[one].age">
        <input type="submit" value="提交"/>
    </form>
    </body>
    </html>
    
    
    ```
    
    
    
  ```java
    
    
    package com.zjs.controller;
    
    import com.zjs.bean.account;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;
    
    @Controller
    @RequestMapping(path = "/user")
    public class HelloController {
    
        @RequestMapping(value = "/hello")
        public  String sayHello(account account)
        {
            System.out.println(account.toString());
            return "success";
        }
    }
    
  ```

* 自定义类型转换器

  * 实现方法

    ```java
    package com.zjs.utils;
    
    
    import org.springframework.core.convert.converter.Converter;
    
    import java.text.DateFormat;
    import java.text.ParseException;
    import java.text.SimpleDateFormat;
    import java.util.Date;
    
    /**
     * 字符串转换为日期
     */
    public class StringToDataConvert implements Converter<String,Date> {
        @Override
        public Date convert(String source) {
            if(source==null)
            {
                throw new RuntimeException("请你传入数据");
            }
            DateFormat dateFormat=new SimpleDateFormat("yyyy-MM-dd");
            try {
    
                return dateFormat.parse(source);
            } catch (Exception e) {
                throw new RuntimeException("转换错误");
            }
        }
    }
    
    ```
  * xml配置
    ```xml
    
     <!--配置自定义转换器-->
        <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
                <set>
                    <bean class="com.zjs.utils.StringToDataConvert"></bean>
                </set>
            </property>
        </bean>
        <!--开启SpringMV框架注解的支持-->
        <mvc:annotation-driven conversion-service="conversionService"></mvc:annotation-driven>
    
    
    
    
    ```



### 1.2 Servlet原生API

* 注解

  * @RequestParam

    ```java
    package com.zjs.controller;
    
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    
    @Controller
    @RequestMapping(value = "/annotation")
    public class AnnotationController {
    
        @RequestMapping(value = "/user")
        public void Annotation(@RequestParam("name") String username)
        {
            System.out.println(username);
        }
    }
     
    ```
    
  * @RequestBody
  
    ```java
    //采用Key-values形式，适用于post请求
    ```
  
  * PathVariable
    
    ```java
    @RequestMapping("/testPahtVariable/{id}")
    public String testPathVariable(@PathVariable(name="id") String id)
    {
    System.out.println(id)
    }
    ```
    
  * @RequestHeader
    
    ```java
    //用于获取头文件
    ```

  * CookieValue
    ```java
    //用于获取cookie值
    ```

* 响应

  * 返回值是void

    ```java
    //编写请求转发的程序
    
    request.getRequestDispatcher("路径").forward(request,response)
    //重定向
       response.sendRedirect(request.getContestPath()+"/index.jsp")
    //设置中文乱码
        response.setCharacterEncoding("utf-8")
        response.setContenttType("text/html;charset=utf-8")
    //直接进行响应
    response.getWriter().print("hello")    
    ```

  * ModelView返回值
  
    ```java
       @RequestMapping(value = "/test")
        public ModelAndView modelAndView()
        {
            ModelAndView mv=new ModelAndView();
            User user=new User();
            user.setAge(10);
            user.setUsername("zjs");
            mv.addObject("user",user);
            mv.setViewName("success");
            return mv;
        }
    ```
  
* 静态资源过滤

  ```xml
      <!--前端控制器，那些静态资源不拦截-->
      <mvc:resources location="/js/" mapping="/js/**"></mvc:resources>
  ```

* json与JavaBean相互转换

  ```xml
  <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.0</version>
  </dependency>
  <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.9.0</version>
  </dependency>
  <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-annotations</artifactId>
      <version>2.9.0</version>
  </dependency
  
  ```

* 前端jsp编写ajax

  ```js
  <script>
  // 页面加载，绑定单击事件
  $(function () {
      $("#btn").click(function () {
          // 发送ajax请求
          $.ajax({
              // 配置请求参数
              url: "user/testAjax",
              contentType: "application/json;charset=UTF-8",
              dataType: "json",
              type: "post",
              // 请求的json数据
              data: '{"username":"myname","password":"mypassowrd","age":30}',
              // 回调函数,处理服务器返回的数据returnData
              success: function (returnData) {
                  // 我们假定服务器返回的是一个user对象,将其输出在控制台上
                  console.log(returnData);            }
          });
      });
  });
  </script>
  
  ```

* 控制器响应json数据

  ```js
  @Controller
  @RequestMapping("/user")
  public class UserController {
      @RequestMapping("/testAjax")
      @ResponseBody
      public User testAjax(@RequestBody User user) {
  
          System.out.println(user);
          
          // 将user对象返回给前端页面
          return user;
      }
  }
  
  ```

  



### 1.3.文件上传





## 2.业务层（spring框架）

## 3.持久层(Mybatis)

