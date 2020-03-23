# IDEA+Mybatis-generator代码生成工具（小白也能用）

## 插件介绍
MyBatis Generator简称MBG，是MyBatis 官方出的代码生成器。MBG能够自动生成实体类、Mapper接口以及对应的XML文件，能够在一定程度上减轻开发人员的工作量。

## 搭建步骤
### 第一步：创建一个maven project
##### 1. 打开IDEA，创建一个maven项目，不用勾选创建模板，点击下一步Next
![创建maven项目](https://img-blog.csdnimg.cn/20200324015720507.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
##### 2. 配置groupId、artifactId和version （自定义）
 ![配置项目的信息](https://img-blog.csdnimg.cn/20200324020420336.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
##### 3. 选择项目存放位置
![选择项目存放位置](https://img-blog.csdnimg.cn/20200324020802744.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
### 第二步：创建数据库的数据表（mysql为例）![创建数据表](https://img-blog.csdnimg.cn/2020032402434532.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)

### 第三步：打开pom.xml引入Mybatis-generator需要的相关依赖，以及IDEA整合Mybatis-generator的插件
#### 1：mybatis依赖：
```java
	<dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.3.2</version>
        </dependency>
```
#### 2：IDEA插件：
```java
 <build>
        <plugins>
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.7</version>
                <dependencies>               
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>5.1.21</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>
```
导入IDEA插件成功后，在右侧maven的工具栏会有mybatis-generator的快捷命令（启动器）
![引入插件](https://img-blog.csdnimg.cn/20200324022738454.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
#### 完整的pom.xml文件

```javascript
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.mybatis</groupId>
    <artifactId>mybatis-generator</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.3.2</version>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.7</version>
                <dependencies>
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>5.1.21</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>

</project>
```

### 第四步：创建配置文件
#### 1：在src/resources目录下新建一个generatorConfig.xml的配置文件，用于配置mybatis-generator的连接信息以及生成的数据；
![创建generatorConfig.xml](https://img-blog.csdnimg.cn/2020032402372992.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
#### 2：配置generatorConfig.xml文件

```javascript
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>

    <!--  mysql数据源配置文件路径  -->
    <properties resource="mysql.properties"/>

    <context id="DB2Tables" targetRuntime="MyBatis3">
        <!--添加这个标签，就证明不会添加注释到代码里面-->
        <!--<commentGenerator>
            <property name="suppressAllComments" value="true" />
        </commentGenerator>-->

        <!--配置数据库连接-->
        <jdbcConnection driverClass="${jdbc.driver}"
                        connectionURL="${db.url}"
                        userId="${db.username}"
                        password="${db.password}">
        </jdbcConnection>

        <javaTypeResolver >
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <!--指定生成javabean的位置-->
        <javaModelGenerator targetPackage="com.dj.pojo" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <!--指定sql的映射文件-->
        <sqlMapGenerator targetPackage="mapper"  targetProject=".\src\main\resources">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>

        <!--指定dao接口生成的位置，mapper接口-->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.dj.mapper"  targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>

        <!--table的生成策略-->
        <!-- <table tableName="ALLTYPES" domainObjectName="Customer" >
             <property name="useActualColumnNames" value="true"/>
             <generatedKey column="ID" sqlStatement="DB2" identity="true" />
             <columnOverride column="DATE_FIELD" property="startDate" />
             <ignoreColumn column="FRED" />
             <columnOverride column="LONG_VARCHAR_FIELD" jdbcType="VARCHAR" />
         </table> -->
        <!--可多写  要生成的表名-->
        <table tableName="brand" domainObjectName="Brand" ></table>


    </context>
</generatorConfiguration>
```
#### 3：配置数据库的连接信息mysql.properties 为了方便修改，所以把配置文件单独提取出来
##### 配置数据库的连接信息

```javascript
#连接数据库的驱动
jdbc.driver=com.mysql.jdbc.Driver

#数据库连接字符串
db.url=jdbc:mysql://127.0.0.1:3306/brand?useUnicode=true&characterEncoding=utf8&useSSL=false
db.username=root
db.password=123456
```
### 第四步：运行mybatis-generator插件，生成mapper，pojo
###### 双击mybatis-generator:generate运行
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200324025029760.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)

# 运行成功后就会在配置的包下生成相关的文件
![完成](https://img-blog.csdnimg.cn/20200324025715302.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pva2VyZGoyMzM=,size_16,color_FFFFFF,t_70)
### 教程结束，仅供参考 还请大神根据不足继续补充
