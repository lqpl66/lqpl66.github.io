---
title: MyBatis Generator 配置
date: 2019-10-29 16:35
author: lqpl66
categories: 
   - springboot
   - 配置
   - mybatis   
tags:  
   - springboot
   - mybatis
---

采用maven配置方法

pom.xml配置其插件

```java
	<!--添加到<build> <plugins> 下面-->
			<plugin>
				<groupId>org.mybatis.generator</groupId>
				<artifactId>mybatis-generator-maven-plugin</artifactId>
				<version>1.3.7</version>
				<configuration>
					<configurationFile>
						${basedir}/src/main/resources/generator/generatorConfig.xml
					</configurationFile>
					<overwrite>true</overwrite>
					<verbose>true</verbose>
				</configuration>
				<dependencies>
					<dependency>
						<groupId>mysql</groupId>
						<artifactId>mysql-connector-java</artifactId>
						<version>5.1.46</version>
					</dependency>
					<dependency>
						<groupId>tk.mybatis</groupId>
						<artifactId>mapper</artifactId>
						<version>3.4.3</version>
					</dependency>
				</dependencies>
			</plugin>
```



```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <context id="Mysql" targetRuntime="MyBatis3Simple" defaultModelType="flat">
        <property name="javaFileEncoding" value="UTF-8"/>
        <property name="useMapperCommentGenerator" value="true"/>

        <plugin type="tk.mybatis.mapper.generator.MapperPlugin">
            <property name="mappers" value="tk.mybatis.mapper.common.Mapper"/>
            <property name="caseSensitive" value="true"/>
            <property name="forceAnnotation" value="true"/>
            <property name="beginningDelimiter" value="`"/>
            <property name="endingDelimiter" value="`"/> 
        </plugin>

        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
			connectionURL="jdbc:mysql://**"
			userId="***"
			password="****">
		</jdbcConnection>

        <!--MyBatis 生成器只需要生成 Model-->
        <javaModelGenerator targetPackage="cn.com.middleware.mapper.po"
                            targetProject="src/main/java">
			<property name="enableSubPackages" value="true" />
			<property name="trimStrings" value="true" />
		</javaModelGenerator>

		<sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
			<property name="enableSubPackages" value="true" />
		</sqlMapGenerator>
		
		<javaClientGenerator type="XMLMAPPER" targetPackage="cn.com.middleware.mapper" targetProject="src/main/java">
			<property name="enableSubPackages" value="true" />
		</javaClientGenerator>
		
        <table tableName="t_updowm_config_middle">
            <generatedKey column="id" sqlStatement="JDBC"/>
        </table>
    </context>
</generatorConfiguration>
```

