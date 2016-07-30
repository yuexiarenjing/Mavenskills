# Mavenskills

Download:
```
http://maven.apache.org/
```

File Structure
```
src
    -main
        -java
            -package
    -test
        -java
            -package
    resources
pom.xml
```

pom.xml
```
<modelVersion>4.0.0</modelVersion>

<groupId></groupId>             #package name
<artifactId></artifactId>       #module name (project name)
<version></version>             #version(SNAPSHOT)

<dependencies>
    <dependency>
        <groupId>junit</groupId>             
        <artifactId>junit</artifactId>      
        <version>4.10</version>             
    </denpendency>
</dependencies>
```



Commands
```
mvn -v                          #show version details
mvn compile                     #compile java project and generate target directory contains classes file
mvn test                        #run debug test
mvn package                     #generate jar

mvn clean                       #delete target directory
mvn install                     #install project into maven repository make other project can use it

mvn archetype:generate          #automate generate maven project
mvn archetype:generate  -DgroupId=组织名，公司网址的反写+项目名
                        -DartifactId=项目名-模块
                        -Dversion=版本号
                        -Dpackage=代码所在的包名

```

坐标
    构件
仓库
    本地仓库和远程仓库
镜像仓库(setting.xml)
更改仓库位置
    可以新建一个maven的repo并将setting.xml文件备份一份在这里
    
#maven的生命周期和插件
maven的eclipse插件
```
新建一个maven project,quick start ,右键pom，run as maven build...，goal填写compile或package命令
```

一个完整的项目构建过程
```
清理    编译    测试    打包    集成测试    验证    部署
```

maven生命周期
```
clean       清理项目(pre-clean  clean post-clean)
default     构建项目(compile test   package install)
site        生成项目站点(pre-site   site    post-site   site-deploy)

以下代码放入pom的project标签内
<build>
  	<plugins>
  		<plugin>
  			<groupId>org.apache.maven.plugins</groupId>
  			<artifactId>maven-source-plugin</artifactId>
  			<version>2.4</version>
  			<executions>
  				<execution>
  					<phase>package</phase>
  					<goals>
  						<goal>jar-no-fork</goal>
  					</goals>
  				</execution>
  			</executions>
  		</plugin>
  	</plugins>
  </build>
```

#pom.xml解析
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  
  <!--  指定当前pom版本 -->
  <modelVersion>4.0.0</modelVersion>

    <groupId>反写的公司网站+项目名</groupId>
    <artifactId>项目名+模块名</artifactId>
    <version>第一个0表示大版本号，分支，小版本0.0.1snapshot/alpha/beta/release/GA正式发布版</version>
    <packaging>不指定默认是jar，还可以是war，zip，pom</packaging>
    <name>项目描述名</name>
    <url>项目的地址</url>
    <descripton></description>
    <developer></developer>
    <license></license>
    <organization></organization>
    
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <type></type>
            <scope>test,只在测试范围内有用（依赖范围）</scope>
            <optional>设置依赖是否可选（true/false）</optional>
            <exclusions>排除依赖传递列表
                <exclusion>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
    
    <dependencyManagement>
        <dependencies>
            <dependency>
        
            </dependency>
        </dependencies>
    </dependencyManagement>
    
    <build>
  	<plugins>
  		<plugin>
  			<groupId>org.apache.maven.plugins</groupId>
  			<artifactId>maven-source-plugin</artifactId>
  			<version>2.4</version>
  			<executions>
  				<execution>
  					<phase>package</phase>
  					<goals>
  						<goal>jar-no-fork</goal>
  					</goals>
  				</execution>
  			</executions>
  		</plugin>
  	</plugins>
  </build>
  
  <parent>对父模块pom的继承</parent>
  
  <modules>
    <module></module>
  </modules>
    
</project>
```
