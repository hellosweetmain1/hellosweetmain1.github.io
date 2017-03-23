### Spring Boot 使用MyBatis自动生成带注解的Mapper

#### pom.xml文件
```xml
<plugin>
	<groupId>org.mybatis.generator</groupId>
	<artifactId>mybatis-generator-maven-plugin</artifactId>
	<version>1.3.2</version>
	<dependencies>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.25</version>
		</dependency>
	</dependencies>
</plugin>
```

#### generatorConfig.xml文件
> resources目录下

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <context id="mysql_tables" targetRuntime="MyBatis3Simple">

        <!-- 去掉mybatis生成的注释 -->
        <commentGenerator>
            <property name="suppressDate" value="true"/>
            <property name="suppressAllComments" value="true" />
        </commentGenerator>

        <!--配置数据库连接信息-->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://127.0.0.1:3306/sweet_mybatis02"
                        userId="root"
                        password="main">
        </jdbcConnection>

        <javaTypeResolver >
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <!--设置要生成的实体类位置-->
        <javaModelGenerator targetPackage="com.peng.entity" targetProject="src/main/java">
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <!--设置要生成的Mapper接口位置-->
        <javaClientGenerator type="ANNOTATEDMAPPER" targetPackage="com.peng.dao" targetProject="src/main/java">
        </javaClientGenerator>

        <!--设置表名对应实体类的名称-->
        <table tableName="student" domainObjectName="Student" >
          <!--这里可以不配置-->
            <generatedKey column="id" sqlStatement="Mysql"/>
        </table>
        <table tableName="teacher" domainObjectName="Teacher" >
            <generatedKey column="id" sqlStatement="Mysql"/>
        </table>

    </context>
</generatorConfiguration>
```

#### 运行Maven命令
> mvn mybatis-generator:generate
