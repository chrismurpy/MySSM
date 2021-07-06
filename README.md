## 1 整合步骤

### 1.1 创建Maven项目

1. 新建Maven项目
2. 在`Project Structure`中的`Modules`中引入`Web`

![1](https://github.com/chrismurpy/MySSM/blob/master/Pic/1.%E5%BC%95%E5%85%A5Module.png)

3. 修改`Web Resource Directories`以及`Deployment Descriptors Path`

   1. 编辑`Path Relative to Deployment Root`，新建`webapp`目录，设置`Path`如下即可。`src/main/webapp`

      ![2](https://github.com/chrismurpy/MySSM/blob/master/Pic/2.Web%20Resource%20Directory.png)

   2. 编辑`Deployment Descriptors`中的`Path`，将`Location`修改为下图所示路径，即将`WEB-INF`目录置于`webapp`下。

      ![3](https://github.com/chrismurpy/MySSM/blob/master/Pic/3.Deployment%20Descriptors.png)

4. 引入`Modules`，如下图所示。

   ![4](https://github.com/chrismurpy/MySSM/blob/master/Pic/4.Artifacts.png)

5. 至此，Maven项目搭建完毕，项目框架如下图所示。

![5](https://github.com/chrismurpy/MySSM/blob/master/Pic/5.Maven%E6%90%AD%E5%BB%BA.png)

### 1.2 pom.xml引入jar依赖

- 面向**静态页面**的SSM依赖：

```xml
<!-- 以war包形式部署于服务器上 -->
<packaging>war</packaging>

<!-- SSM整合依赖：Spring-SpringMVC Mybatis Mysql -->
<!-- 集中定义依赖版本号：注意版本号，避免踩坑 -->
<properties>
    <spring.version>5.2.13.RELEASE</spring.version>
    <mybatis.version>3.5.6</mybatis.version>
    <mybatis.spring.version>1.3.3</mybatis.spring.version>
    <pagehelper.version>5.2.1</pagehelper.version>
    <mysql.version>8.0.16</mysql.version>
    <druid.version>1.2.6</druid.version>
    <servlet-api.version>4.0.1</servlet-api.version>
    <jackson.version>2.9.6</jackson.version>
    <log4j.version>1.2.17</log4j.version>
    <junit.version>4.12</junit.version>
</properties>
<dependencies>
    <!-- Spring -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
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
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!-- 文件上传 -->
    <dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.4</version>
    </dependency>
    <!-- Mybatis -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>${mybatis.version}</version>
    </dependency>
    <!-- Mybatis-Spring整合 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>${mybatis.spring.version}</version>
    </dependency>
    <!-- 分页插件 -->
    <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper</artifactId>
        <version>${pagehelper.version}</version>
    </dependency>
    <!-- MySQL -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>${mysql.version}</version>
    </dependency>
    <!-- Druid连接池 -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>${druid.version}</version>
    </dependency>
    <!-- servlet -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>${servlet-api.version}</version>
        <scope>provided</scope>
    </dependency>
    <!-- Jackson - JSON处理工具包 -->
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>${jackson.version}</version>
    </dependency>
    <!-- Log4j -->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>${log4j.version}</version>
    </dependency>
    <!-- Junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
        <scope>test</scope>
    </dependency>
    <!-- Spring测试 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!-- @Resource注解依赖 -->
    <dependency>
        <groupId>javax.annotation</groupId>
        <artifactId>javax.annotation-api</artifactId>
        <version>1.3.2</version>
    </dependency>
</dependencies>

<!-- 插件配置 -->
<build>
    <resources>
        <!-- 静态资源过滤 -->
        <resource>
            <!-- 所在的目录 -->
            <directory>src/main/java</directory>
            <!-- 包括目录下的.properties，.xml文件都会被扫描 -->
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
    <plugins>
        <!-- 设置项目的编译版本 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
        <!-- 设置Tomcat插件 -->
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
            <configuration>
                <!-- 指定端口 -->
                <port>8080</port>
                <!-- 请求路径 -->
                <path>/</path>
                <uriEncoding>UTF-8</uriEncoding>
            </configuration>
        </plugin>
        <!-- 反向生成插件 -->
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.5</version>
            <!-- 配置文件的路径 -->
            <configuration>
                <configurationFile>src/main/resources/generatorConfig.xml</configurationFile>
                <overwrite>true</overwrite>
            </configuration>
            <dependencies>
                <dependency>
                    <groupId>org.mybatis.generator</groupId>
                    <artifactId>mybatis-generator-core</artifactId>
                    <version>1.3.5</version>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>
</build>
```


### 1.3 编写Mybatis的配置文件

Mybatis的配置文件`mybatis.xml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <setting name="logImpl" value="LOG4J"/>
    </settings>
    <!-- Spring接管其他的工作 - 数据源/映射文件注册/插件 -->
</configuration>
```

日志配置文件文件`log4j.properties`

```properties
# Global logging configuration  info  warning  error
log4j.rootLogger=DEBUG,stdout
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

### 1.4 编写Spring的配置文件spring.xml

连接数据库的参数配置文件`jdbc.properties`

```properties
# 数据库版本 MySQL8
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://127.0.0.1:3306/SSM_NBA?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=Asia/Shanghai
jdbc.username=murphy
jdbc.password=xmf199315
```

`spring.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/tx
       https://www.springframework.org/schema/tx/spring-tx.xsd">

    <!-- 这些包添加注解后，对象的创建权限便交给Spring容器 -->
    <context:component-scan base-package="com.murphy.mapper,com.murphy.service"/>

    <!-- Spring整合Mybatis -->
    <context:property-placeholder location="classpath*:jdbc.properties"/>
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <!-- 驱动可以省略，根据URL自行判定 -->
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 如果有mybatis的单独的配置文件，需要在此引入 -->
        <property name="configLocation" value="classpath:mybatis.xml"/>
        <property name="dataSource" ref="dataSource"/>
        <!-- 配置别名 -->
        <property name="typeAliasesPackage" value="com.murphy.pojo"/>
        <!-- 配置映射文件 -->
        <property name="mapperLocations" value="com/murphy/mapper/*.xml"/>
        <!-- 配置插件 -->
        <property name="plugins">
            <array>
                <!-- 分页 -->
                <bean class="com.github.pagehelper.PageInterceptor">
                    <property name="properties">
                        <value>
                            reasonable=true
                        </value>
                    </property>
                </bean>
            </array>
        </property>
    </bean>
    <!-- 接口产生动态代理类 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.murphy.mapper"/>
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
    </bean>

    <!-- 通过注解实现事务 -->
    <tx:annotation-driven transaction-manager="transactionManager"/>
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
</beans>
```

### 1.5 编写SpringMVC的配置文件springmvc.xml

`springmvc.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- SpringMVC的配置文件：控制器的bean对象在此处扫描 -->
    <context:component-scan base-package="com.murphy.controller"/>
    <mvc:annotation-driven/>
    <!-- 视图解析器 -->
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/pages/"/>
        <property name="suffix" value=".html"/>
    </bean>
    <!-- 静态资源处理 -->
    <mvc:resources mapping="/img/**" location="/img/"/>
    <mvc:resources mapping="/js/**" location="/js/"/>
    <mvc:resources mapping="/css/**" location="/css/"/>
    <mvc:resources mapping="/fonts/**" location="/fonts/"/>
    <mvc:resources mapping="/static/**" location="/static/"/>
    <mvc:resources mapping="/pages/**" location="/pages/"/>

    <!-- 文件上传 -->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    </bean>
</beans>
```

### 1.6 导入静态资源到webapp文件下

### 1.7 编写web.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <welcome-file-list>
        <welcome-file>/pages/index.html</welcome-file>
    </welcome-file-list>

    <!-- Spring的配置 -->
    <context-param>
        <!--
            contextConfigLocation：表示用于加载Bean的配置文件
            classpath / classpath* 区别：
                classpath：只会到你的class路径中查找文件
                classpath*：不仅包含class路径，还包括jar文件中(class路径)进行查找
        -->
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath*:spring.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!-- SpringMVC的配置 -->
    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 创建前端控制器的时候去读SpringMVC配置文件启动IoC容器 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath*:springmvc.xml</param-value>
        </init-param>
        <!-- Tomcat启动便创建此对象 -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!-- 配置拦截路径URL，所有请求都会被前端控制器拦截处理 -->
    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--
        使用Rest风格的URI 将页面普通的POST请求转换为指定的delete或者put请求
        原理：在Ajax中发送POST请求后，带_method参数，将其修改为PUT，或者DELETE请求
    -->
    <filter>
        <filter-name>httpMethodFilter</filter-name>
        <filter-class>
            org.springframework.web.filter.HiddenHttpMethodFilter
        </filter-class>
    </filter>
    <filter-mapping>
        <filter-name>httpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!-- 注册字符集过滤器：POST请求中文乱码问题的解决方法 -->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!-- 指定字符集 -->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <!-- 强制request使用字符集encoding -->
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <!-- 强制response使用字符集 -->
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

### 1.8 测试

<img src="/Users/murphy/Desktop/Typora/Pic/2. Maven运行Tomcat-5561320.png" style="zoom:50%;" />

启动项目，浏览器访问`http://localhost:8080/`。看到页面表示项目整合成功。

- 至此，SSM整合完毕，项目结构如图所示：

![6](https://github.com/chrismurpy/MySSM/blob/master/Pic/6.SSM%E6%90%AD%E5%BB%BA.png)

------

## 
