---
layout: post
title: CentOS 7 部署 maven 最新版运行 springboot项目
date: 2019-10-23
tags: 管理工具
---


### Maven介绍
   
 　 [maven官网](https://maven.apache.org/)
 
> * 简化构建过程
> * 提供统一的构建系统
> * 提供优质的项目信息
> * 提供最佳实践开发指南
> * 允许透明迁移到新功能

###  安装maven

 　[maven下载地址](https://mirrors.tuna.tsinghua.edu.cn/apache/maven/)

```     
$ wget https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.2/binaries/apache-maven-3.6.2-bin.tar.gz
$ tar -xzvf apache-maven-3.6.2-bin.tar.gz -C /usr/local
$ mv /usr/local/apache-maven-3.6.2/ /usr/local/maven/

$ vi /etc/profile
MAVEN_HOME=/usr/local/maven
export MAVEN_HOME
export PATH=$PATH:$MAVEN_HOME/bin
$ source /etc/profile
```    

###  运行springboot项目

 　springboot项目测试地址：https://github.com/xiaobingchan/jenkinsdemo
   
```    
$ git clone https://github.com/xiaobingchan/jenkinsdemo.git

$ cd jenkinsdemo/

$ mvn clean
[INFO] Scanning for projects...
[INFO]
[INFO] --------------< com.bolingcavalry:mavendockerplugindemo >---------------
[INFO] Building mavendockerplugindemo 0.0.1-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-clean-plugin/2.6.1/maven-clean-plugin-2.6.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-clean-plugin/2.6.1/maven-clean-plugin-2.6.1.pom (5.0 kB at 1.5 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-plugins/26/maven-plugins-26.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-plugins/26/maven-plugins-26.pom (11 kB at 20 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-parent/25/maven-parent-25.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/maven-parent/25/maven-parent-25.pom (37 kB at 29 kB/s)
[INFO]
[INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ mavendockerplugindemo ---
Downloading from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-utils/1.1/plexus-utils-1.1.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-utils/1.1/plexus-utils-1.1.jar (169 kB at 104 kB/s)
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  13.523 s
[INFO] Finished at: 2019-10-28T21:21:33+08:00
[INFO] ------------------------------------------------------------------------

$ mvn package

Downloaded from central: https://repo.maven.apache.org/maven2/asm/asm-analysis/3.2/asm-analysis-3.2.jar (18 kB at 693 B/s)
Downloaded from central: https://repo.maven.apache.org/maven2/commons-io/commons-io/1.3.2/commons-io-1.3.2.jar (88 kB at 3.4 kB/s)
Downloaded from central: https://repo.maven.apache.org/maven2/asm/asm-util/3.2/asm-util-3.2.jar (37 kB at 1.4 kB/s)
Downloaded from central: https://repo.maven.apache.org/maven2/org/sonatype/sisu/sisu-guice/2.1.7/sisu-guice-2.1.7-noaop.jar (472 kB at 18 kB/s)
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  22:25 min
[INFO] Finished at: 2019-10-28T22:47:34+08:00
[INFO] ------------------------------------------------------------------------

```    

   第一次打包maven仓库，用了23分钟，这里为了节约大家下载jar包初始化的时间，我把我的jar包仓库放到了我的 [git仓库](https://github.com/xiaobingchan/maven_resp)，下面我们运行已经编译完成的jar包
   
```  
[root@VM_0_12_centos jenkinsdemo]# java -jar target/mavendockerplugindemo-0.0.1-SNAPSHOT.jar

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v1.5.9.RELEASE)

2019-10-28 23:53:13.928  INFO 19542 --- [           main] c.b.m.MavendockerplugindemoApplication   : Starting MavendockerplugindemoApplication v0.0.1-SNAPSHOT on VM_0_12_centos with PID 19542 (/root/jenkinsdemo/target/mavendockerplugindemo-0.0.1-SNAPSHOT.jar started by root in /root/jenkinsdemo)
2019-10-28 23:53:13.942  INFO 19542 --- [           main] c.b.m.MavendockerplugindemoApplication   : No active profile set, falling back to default profiles: default
2019-10-28 23:53:14.134  INFO 19542 --- [           main] ationConfigEmbeddedWebApplicationContext : Refreshing org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@5387f9e0: startup date [Mon Oct 28 23:53:14 CST 2019]; root of context hierarchy
2019-10-28 23:53:17.713  INFO 19542 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat initialized with port(s): 8080 (http)
2019-10-28 23:53:17.749  INFO 19542 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2019-10-28 23:53:17.750  INFO 19542 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet Engine: Apache Tomcat/8.5.23
2019-10-28 23:53:17.985  INFO 19542 --- [ost-startStop-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2019-10-28 23:53:17.985  INFO 19542 --- [ost-startStop-1] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 3854 ms
2019-10-28 23:53:18.305  INFO 19542 --- [ost-startStop-1] o.s.b.w.servlet.ServletRegistrationBean  : Mapping servlet: 'dispatcherServlet' to [/]
2019-10-28 23:53:18.317  INFO 19542 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'characterEncodingFilter' to: [/*]
2019-10-28 23:53:18.317  INFO 19542 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'hiddenHttpMethodFilter' to: [/*]
2019-10-28 23:53:18.317  INFO 19542 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'httpPutFormContentFilter' to: [/*]
2019-10-28 23:53:18.317  INFO 19542 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'requestContextFilter' to: [/*]
2019-10-28 23:53:19.127  INFO 19542 --- [           main] s.w.s.m.m.a.RequestMappingHandlerAdapter : Looking for @ControllerAdvice: org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@5387f9e0: startup date [Mon Oct 28 23:53:14 CST 2019]; root of context hierarchy
2019-10-28 23:53:19.311  INFO 19542 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/]}" onto public java.lang.String com.bolingcavalry.mavendockerplugindemo.controller.Hello.sayHello()
2019-10-28 23:53:19.319  INFO 19542 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error],produces=[text/html]}" onto public org.springframework.web.servlet.ModelAndView org.springframework.boot.autoconfigure.web.BasicErrorController.errorHtml(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)
2019-10-28 23:53:19.319  INFO 19542 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error]}" onto public org.springframework.http.ResponseEntity<java.util.Map<java.lang.String, java.lang.Object>> org.springframework.boot.autoconfigure.web.BasicErrorController.error(javax.servlet.http.HttpServletRequest)
2019-10-28 23:53:19.399  INFO 19542 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2019-10-28 23:53:19.399  INFO 19542 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2019-10-28 23:53:19.509  INFO 19542 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**/favicon.ico] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2019-10-28 23:53:19.811  INFO 19542 --- [           main] o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
2019-10-28 23:53:20.052  INFO 19542 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat started on port(s): 8080 (http)
2019-10-28 23:53:20.058  INFO 19542 --- [           main] c.b.m.MavendockerplugindemoApplication   : Started MavendockerplugindemoApplication in 7.043 seconds (JVM running for 7.94)

```     
 
   访问  http://127.0.0.1:8080/ 
```     
[root@VM_0_12_centos jenkinsdemo]# curl http://127.0.0.1:8080/
123456. Hello jenkins, Mon Oct 28 23:56:02 CST 2019
[root@VM_0_12_centos jenkinsdemo]# 
```     