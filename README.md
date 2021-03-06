# boot-jeesite
开源框架JeeSite集成SpringBoot实例

## boot-jeesite

 本项目衍生自 
 [jeesite](https://github.com/thinkgem/jeesite)、
 [开源框架JeeSite集成SpringBoot实例](https://www.cnblogs.com/frightOAO/p/7857743.html)
 参考
 [one](https://github.com/lcw2004/one)
 [renren-fast](https://gitee.com/babaio/renren-fast)
 [L316476844/springbootexample](https://github.com/L316476844/springbootexample)
 [Foreveriss/SpringBoot](https://github.com/Foreveriss/SpringBoot)
 
 在sb实例的基础上，添加了原有的内容管理（CMS）、模块代码生成（GEN）模块
 
## 技术栈
- 后端
    -   Spring Boot 1.5.6.RELEASE
    -   使用SSM框架, Spring+Spring MVC+MyBatis3.2.8

- 前端
    -   xxx
    
        
## 启动
- jsp 开发模式（含代码生成， cms），需配置 tomcat 启动，打成 war 包，tomcat 部署。   
    - http://localhost:8087   登录页    
- 前后端模式后端，可去除支持 jsp 的相关支持，springboot 启动类启动，打成 jar 包部署。   
    - http://localhost:8087/test/  返回测试api数据
 
 
## 2018-08-16

1. 服务调用 SA->SB , resttemplate getForEntity 
2. root 日志级别，当前为 error，info 输出 tomcat port  

 
## 2018-08-09

1. Eureka 不要关闭自我保护，多 e-server 开启自我注册   

- eureka-server 
准备单机情况下： host 修改 添加 127.0.0.1 对应 master slave1  slave2   
```
master

java -jar boot-jeesite-cloud-eureka-1.0.jar --server.port=8761 --eureka.instance.hostname=master --eureka.client.serviceUrl.defaultZone=http://slave1:8762/eureka/,http://slave2:8763/eureka/  

slave1
java -jar boot-jeesite-cloud-eureka-1.0.jar  --server.port=8762 --eureka.instance.hostname=slave1 --eureka.client.serviceUrl.defaultZone=http://master:8761/eureka/,http://slave2:8763/eureka/  

slave2
java -jar boot-jeesite-cloud-eureka-1.0.jar --server.port=8763 --eureka.instance.hostname=slave2 --eureka.client.serviceUrl.defaultZone=http://master:8761/eureka/,http://slave1:8762/eureka/  
```
- service 服务
服务 A 调用 B

```
service A  

java -jar boot-jeesite-cloud-serviceA-1.0.jar --server.port=8093 --eureka.instance.appname=cloud-service-A --spring.application.name=cloud-service-A --eureka.client.serviceUrl.defaultZone=http://master:8761/eureka/,http://slave1:8762/eureka/,http://slave2:8763/eureka/

service B

java -jar boot-jeesite-cloud-serviceB-1.0.jar --server.port=8097 --eureka.instance.appname=cloud-service-B --spring.application.name=cloud-service-B --eureka.client.serviceUrl.defaultZone=http://master:8761/eureka/,http://slave1:8762/eureka/,http://slave2:8763/eureka/
```

2. 服务 A 调用 B 暂未实现


3. 问题
1) host slave1 可以，slave_1 下划线形式不可以（mac）eureka 服务启动报错，`freemaker xxx xxx unknown host:xxxx`  
答： 特意查了一下，大意是域名不运行存在的字符含 下划线 [refer to](https://blog.csdn.net/codejoker/article/details/5367331)  

2) 多模块，打包问题，引用顺序--报错提示没有找到main函数  
别引用模块不要引入 spring-boot-maven-plugin插件  
[refer to](https://blog.csdn.net/lizhongfu2013/article/details/79656972).   


   
    
## 2018-07-02  
1. oss 实现（七牛云） 
2. 准备集成 cloud  
    
## 2018-06-06  
1. docker-compose 配置添加 nginx  
2. fastdfs 上传 demo （webuploader）  
3. 分离 configer moudule  
4. freemarker 生成静态页 demo  
5. oss 暂未实现（七牛云）  
    

## 2018-05-25
1. docker-compose fastDFS   
[refer to ](https://github.com/luhuiguo/fastdfs-docker)  
[refer to 使用docker-compose一键部署分布式文件系统FastDFS](http://www.yunwzs.com/1910.html) 
    
## 2018-05-23  
1. docker-compose 配置 zipkin+elasticsearch     
2. 添加 ZipkinConfig 配置类，pom 添加 zipkin 依赖  
  
## 2018-05-18  
1. 拆分多模块开发模式  
2. 先把 gen 生产代码剥离出来，还用原来的 jsp 页面很是经典  
3. 还存在问题，之后解决    
- 问题记录： 
1. 健康监控 `spring-boot-starter-actuator` 配置文件不设置开辟单独端口， 
与 拦截器 `WebMvcConfigurerAdapter` 冲突  
2. `druid-starter` 配置允许`multi-statement-allow` 支持sql批量操作  
3. 代码生成刚好用 springboot 支持 jsp 的模式 ， 就在 jeesite-gen-web 模块  

## 2018-05-16  
1. global,读取属性文件配置 RelaxedPropertyResolver 实现  
2. 配置缓存shiro redis，分布式部署共享 redis 存储 sessionId
3. ShiroConfiguration 配置简化，去掉@Bean 方法传参方式
- 问题记录：   
1. 去掉热部署，使用热部署 shiro+redis 时报错：`java.lang.ClassCastException`  
`Principal principal = (Principal) subject.getPrincipal();`  
三种解决办法，这里去掉采用去掉热更新配置   
[refer to :](https://blog.csdn.net/zhaoyachao123/article/details/79413908)    
2. ShiroConfiguration @Value获取值为 null  
`public static LifecycleBeanPostProcessor`此处设置为 `static` 可行  
[原因及解决 refer to :](https://stackoverflow.com/questions/31388445/apache-shiro-jdbcrealm-with-javaconfig-and-spring-boot)  

    
## 2018-05-10  
1. shiro 配置支持移动端，head 添加 Authorization 存储 sessionid   
2. shiro 升级1.3.2 配置取消 url 携带 sessionId
3. 修改登录 controller 统一返回类型 resultMode ，移动端认证成功返回 token   

## 2018-05-08  
1. 添加 api 返回标准格式   
2. 添加全局异常处理  
3. 删除 supcan，static  
    
## 2018-05-05

+ f, 
1. bean 配置 ehCache 
2. 修改默认数据库 url 
3. 添加单元测试
4. 修复启动单元测试或者 `maven test` shiroCache cacheManger 注入多个
5. 添加 swagger api 接口文档
6. 配置类全局跨域
7. shiro filter 暂无权限限制
    
## 2018-05-02

0. 支持多数据源（spring boot+druid+mybatis+多数据源），重点参考2     
1. 没有采用 aop 切换数据源。人为再 controller 层控制数据源选择。    
2. 没有采用简单负载，只是针对不同数据源非主从模式，determineCurrentLookupKey 默认 db1 数据源  
[参考1](http://www.cnblogs.com/yjmyzz/p/spring-boot-integrate-with-mybatis-and-multi-datasource.html)  
[参考2](https://github.com/drtrang/druid-spring-boot)  
       
       
## 2018-04-27  
0.解决打成的 jar 包过大问题，便于后期部署或者 docker 集成  
1.maven plugin 设置 jar 分离 lib 'thin jar'  
[refer](https://my.oschina.net/xiaozhutefannao/blog/1624092)  

   
## 2018-04-11
   
0. 修改启动后自动访问 '/'( 去除 yml 内 springMVC index 配置项 , 非 sb 项目在 springMVC 配置文件)    
1. 配置 druid
2. 日志文件路径 定位到项目路径内 logs ehcache
3. modules 添加 一个简单测试 controller
    
## to-do-list
