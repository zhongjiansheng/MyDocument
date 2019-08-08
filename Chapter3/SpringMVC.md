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

* jar包导入

  ```xml
  <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.1</version>
  </dependency>
  <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.4</version>
  </dependency>
  
  ```

* 文件上传的三种实现

  * 使用JavaEE方式

    ```java
    @Controller
    @RequestMapping("/fileUpload")
    public class FileUploadController {
    
    	@RequestMapping("/javaEE")
        public String fileupload1(HttpServletRequest request) throws Exception {
      
            // 创建目录保存上传的文件
            String path = request.getSession().getServletContext().getRealPath("/uploads/");
            File file = new File(path);
            if (!file.exists()) {
                file.mkdirs();
            }
    
            // 创建ServletFileUpload来解析request
            ServletFileUpload upload = new ServletFileUpload(new DiskFileItemFactory());
            List<FileItem> items = upload.parseRequest(request);
            // 遍历解析的结果,寻找上传文件项
            for (FileItem item : items) { 
                if (!item.isFormField()) {
                    // 不是普通表单项,说明是文件上传项
                    
                    // 服务器中保存的文件名
                    String filename = UUID.randomUUID().toString().replace("-", "") + "_" + item.getName();
                    // 上传文件
                    item.write(new File(path, filename));
                    // 删除临时文件
                    item.delete();
                }
            }
    
            return "success";
        }
    }
    
    ```

  * 使用SpringMVC进行单服务器上传

    ```xml
    <!-- 配置文件解析器,其id是固定的,必须为multipartResolver -->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!-- 设置文件的最大尺寸 -->
        <property name="maxUploadSize" value="10485760"/>
    </bean>
    
    ```

    ```java
    @Controller
    @RequestMapping("/fileUpload")
    public class FileUploadController {
    
    	@RequestMapping("/springMVC")
        public String fileupload2(HttpServletRequest request, @RequestParam("fileParam") MultipartFile upload) throws Exception {
            
            // 创建目录保存上传的文件
            String path = request.getSession().getServletContext().getRealPath("/uploads/");
            File file = new File(path);
            if (!file.exists()) {
                file.mkdirs();
            }
    
            // 服务器中保存的文件名
            String filename = UUID.randomUUID().toString().replace("-", "") + "_" + upload.getOriginalFilename();
            // 上传文件
            upload.transferTo(new File(path,filename));
    
            return "success";
        }
    }
    
    ```

  * SpringMVC跨服务器

    ```xml
    <dependency>
        <groupId>com.sun.jersey</groupId>
        <artifactId>jersey-core</artifactId>
        <version>1.18.1</version>
    </dependency>
    <dependency>
        <groupId>com.sun.jersey</groupId>
        <artifactId>jersey-client</artifactId>
        <version>1.18.1</version>
    </dependency>
    
    ```

    ```java
    @Controller
    @RequestMapping("/fileUpload")
    public class FileUploadController {
    
    	@RequestMapping("/betweenServer")
        public String fileupload3(@RequestParam("fileParam") MultipartFile upload) throws Exception {
            System.out.println("跨服务器文件上传...");
    
            // 文件服务器URL
            String fileServerPath = "http://localhost:9090/uploads/";	
            
            // 获取服务器中保存的文件名
            String filename = UUID.randomUUID().toString().replace("-", "") + "_" + upload.getOriginalFilename();
    
            // 创建客户端对象并在文件服务器上创建资源
            Client client = Client.create();
            WebResource webResource = client.resource(fileServerPath + filename);
            webResource.put(upload.getBytes());
    
            return "success";
        }
    }
    
    ```
  
* 处理文件的工具类
  
  ```java
  //将上述程序中对文件的处理封装成抽象类FileUtil:
  package cn.maoritian.utils;
  
  import java.io.File;
  import java.io.FileOutputStream;
  import java.util.UUID;
  
  public class FileUtil {
  
      // 上传文件
      public static void uploadFile(byte[] file, String filePath, String fileName) throws Exception {
          File targetFile = new File(filePath);
          if (!targetFile.exists()) {
              targetFile.mkdirs();
          }
          FileOutputStream out = new FileOutputStream(filePath + fileName);
          out.write(file);
          out.flush();
          out.close();
      }
  
      // 删除文件,返回值表示是否删除成功
      public static boolean deleteFile(String fileName) {
          File file = new File(fileName);
          // 如果文件路径所对应的文件存在，并且是一个文件，则直接删除
          if (file.exists() && file.isFile()) {
              if (file.delete()) {
                  return true;
              } else {
                  return false;
              }
          } else {
              return false;
          }
      }
  
      // 重命名文件
      public static String renameToUUID(String fileName) {
          return UUID.randomUUID() + "." + fileName.substring(fileName.lastIndexOf(".") + 1);
      }
  }
  
  
  //保存文件的操作可简化为如下三句:
  String fileName = FileUtil.renameToUUID(uploadFile.getOriginalFilename());
  String filePath = request.getSession().getServletContext().getRealPath("/uploads/");
  FileUtil.uploadFile(uploadFile.getBytes(), filePath, fileName);
  
  ```
  
  
### 1.4.异常处理

* 自定义异常处理类

  ```java
  package com.zjs.utils;
  
  public class MyException extends Exception {
      private String errmsg;
  
      public MyException(String errmsg)
      {
          super(errmsg);
      }
      public String getErrmsg() {
          return errmsg;
      }
  
      public void setErrmsg(String errmsg) {
          this.errmsg = errmsg;
      }
  }
  
  ```

* 抛出异常

  ```java
     @RequestMapping(value = "/testException")
      public String testException() throws MyException {
          try {
              int z=1/0;
          }catch (Exception e)
          {
              e.printStackTrace();
              throw new MyException("testException抛出异常了");
          }
  
          return "success";
      }
  ```

* 异常处理器

  ```java
  package com.zjs.utils;
  
  import org.springframework.web.servlet.HandlerExceptionResolver;
  import org.springframework.web.servlet.ModelAndView;
  
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  
  /**
   * 异常处理器对象
   */
  public class MyExceptionResolver implements HandlerExceptionResolver {
      @Override
      public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
  
          //获取异常对象
          MyException my=null;
          if(e instanceof MyException)
          {
              my=(MyException)e;
          }
          else {
              my=new MyException("系统正在维护....");
          }
          ModelAndView modelAndView=new ModelAndView();
          modelAndView.addObject("errmsg", my.getMessage());
          modelAndView.setViewName("error");
          return modelAndView;
  
      }
  }
  
  ```

* 异常处理器需加入组件当中

  ```xml
  <bean id="myexceptionresolver" class="com.zjs.utils.MyExceptionResolver"></bean>
  
  ```

  
### 1.5.拦截器

* 实现拦截类

  ```java
  package com.zjs.utils;
  
  import org.springframework.web.servlet.HandlerInterceptor;
  import org.springframework.web.servlet.ModelAndView;
  
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  
  public class MyInter implements HandlerInterceptor {
      /**
       * 参与护理，controller方法执行前
       * return true 放行，执行下一个拦截器，如果没有，执行controller方法
       * return false 不放行
       * @param request
       * @param response
       * @param handler
       * @return
       * @throws Exception
       */
      @Override
      public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
          System.out.println("执行力拦截器");
          return true;
      }
  
      /**
       * 页面执行后，该方法会执行
       * @param request
       * @param response
       * @param handler
       * @param ex
       * @throws Exception
       */
      @Override
      public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
  
      }
  
      /**
       * 后处理方法 controller方法执行后，success.jsp之前
       * @param request
       * @param response
       * @param handler
       * @param modelAndView
       * @throws Exception
       */
      @Override
      public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
  
      }
  }
  
  ```

* 配置拦截器

  ```xml
      <!--配置拦截器-->
      <mvc:interceptors>
          <mvc:interceptor>
              <!--要拦截的请求路径-->
              <mvc:mapping path="/user/*"/>
              <!--不用拦截的请求路径-->
              <!--<mvc:exclude-mapping path=""></mvc:exclude-mapping>-->
              <!--拦截器对象-->
              <bean class="com.zjs.utils.MyInter"></bean>
          </mvc:interceptor>
      </mvc:interceptors>
  ```

  

## 2.业务层（spring框架）

## 3.持久层(Mybatis)

## 4.ssm整合

#### 4.1.pom文件

```xml
<properties>
		<spring.version>5.0.2.RELEASE</spring.version>
		<slf4j.version>1.6.6</slf4j.version>
		<log4j.version>1.2.12</log4j.version>
		<mysql.version>5.1.6</mysql.version>
		<mybatis.version>3.4.5</mybatis.version>
	</properties>

	<dependencies>

		<!-- spring -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>1.6.8</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
			<version>${spring.version}</version>
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
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>${mysql.version}</version>
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

		<dependency>
			<groupId>jstl</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>

		<!-- log start -->
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>${log4j.version}</version>
		</dependency>

		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${slf4j.version}</version>
		</dependency>

		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${slf4j.version}</version>
		</dependency>
		<!-- log end -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>${mybatis.version}</version>
		</dependency>

		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.3.0</version>
		</dependency>

		<dependency>
			<groupId>c3p0</groupId>
			<artifactId>c3p0</artifactId>
			<version>0.9.1.2</version>
			<type>jar</type>
			<scope>compile</scope>
		</dependency>

	</dependencies>
<build>
		<finalName>ssm</finalName>
		<pluginManagement>
			<plugins>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-compiler-plugin</artifactId>
					<version>3.2</version>
					<configuration>
						<source>1.8</source>
						<target>1.8</target>
						<encoding>UTF-8</encoding>
						<showWarnings>true</showWarnings>
					</configuration>
				</plugin>
			</plugins>
		</pluginManagement>
	</build>
	
```

### 4.2.aplicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd">
    <!-- 配置 spring 创建容器时要扫描的包 -->
    <context:component-scan base-package="com.zjs">
    <!--制定扫包规则，不扫描@Controller 注解的 JAVA 类，其他的还是要扫描 -->
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    </context:component-scan>

</beans>

```

### 4.3.springmvc.xml

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">
     <!--开启注解扫描-->
    <context:component-scan base-package="com.zjs">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"></context:include-filter>
    </context:component-scan>
    <!--视图解析器对象-->
    <bean id="internalResouceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--配置文件所在目录-->
        <property name="prefix" value="/WEB-INF/pages/"></property>
        <!--配置后缀名-->
        <property name="suffix" value=".jsp"></property>
    </bean>
    <!--配置自定义转换器-->
    <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="com.zjs.utils.StringToDataConvert"></bean>
            </set>
        </property>
    </bean>
    <!--配置文件解析器,并且id名称必须是multipartResolver-->
    <bean id="multiparResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="10485760"></property>
     </bean>
    <!--配置异常处理器对象-->
    <bean id="myexceptionresolver" class="com.zjs.utils.MyExceptionResolver"></bean>
    <!--配置拦截器-->
    <mvc:interceptors>
        <mvc:interceptor>
            <!--要拦截的请求路径-->
            <mvc:mapping path="/user/*"/>
            <!--不用拦截的请求路径-->
            <!--<mvc:exclude-mapping path=""></mvc:exclude-mapping>-->
            <!--拦截器对象-->
            <bean class="com.zjs.utils.MyInter"></bean>
        </mvc:interceptor>
    </mvc:interceptors>
    <!--前端控制器，那些静态资源不拦截-->
    <mvc:resources location="/js/" mapping="/js/**"></mvc:resources>
    <!--开启SpringMV框架注解的支持-->
    <mvc:annotation-driven conversion-service="conversionService"></mvc:annotation-driven>

</beans>
```

### 4.4.web.xml

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
  <!--配置解决乱码的过滤器-->
  <filter>
    <filter-name>characterEncoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>  
  </filter>
  <filter-mapping>
    <filter-name>characterEncoding</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
</web-app>

```



### 4.5.整合Spring和SpringMVC

```xml
<!-- 配置 spring 提供的监听器，用于启动服务时加载容器 。
该间监听器只能加载 WEB-INF 目录中名称为 applicationContext.xml 的配置文件 --> <listener><listener-class>
org.springframework.web.context.ContextLoaderListener
</listener-class>
</listener>
<!-- 手动指定 spring 配置文件位置 --> <context-param> <param-name>contextConfigLocation</param-name> <param-value>classpath:applicationContext.xml</param-value>
</context-param>
```





