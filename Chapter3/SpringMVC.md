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
        <!--开启SpringMV框架注解的支持-->
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

## 2.业务层（spring框架）

## 3.持久层(Mybatis)

