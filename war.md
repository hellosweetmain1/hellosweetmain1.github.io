### Spring Boot 打包WAR
1. pom.xml

 ```xml
 <packaging>war</packaging>

 <dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-tomcat</artifactId>
 <scope>provided</scope>
 </dependency>
 ```
2. 修改启动类
 ```java
 @SpringBootApplication
 public class WardemoApplication extends SpringBootServletInitializer {

  public static void main(String[] args) {
		SpringApplication.run(WardemoApplication.class, args);
	}

	@Override
	protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
		return application.sources(WardemoApplication.class);
	}

 }
 ```
3. 生成 WAR包
 ```java
 // 在项目目录下运行命令行, 在target目录中生成war包
 mvn package
 ```
4. 部署到Tomcat
 > 把war包放到 webapps下， 然后启动Tomcat。
