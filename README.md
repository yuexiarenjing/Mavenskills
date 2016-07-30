# Mavenskills

	管理项目的依赖和构建过程

Download:
```
http://maven.apache.org/
```

Link:
```
https://mvnrepository.com/
http://tomcat.apache.org/
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

```
坐标
    构件
仓库
    本地仓库和远程仓库
镜像仓库(setting.xml)
更改仓库位置
    可以新建一个maven的repo并将setting.xml文件备份一份在这里
```

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
#依赖的范围、传递、继承
依赖的范围
```
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
      <!-- 
        compile，默认的，编译、测试、运行三种classpath都有效
        provided，编译、测试
        runtime，测试、运行有效
        test,测试
        system，编译、测试
        import，导入依赖
      -->
    </dependency>
  </dependencies>
```

依赖传递
```
    A依赖B，B依赖C，则A依赖于C
    B和C需要install进本地repo
    若A不想依赖于C，可以在添加对B的依赖时<exclusions><exclusion>+C的定位信息...来取消对C的依赖
```

依赖冲突
```
短路优先：依赖传递的次数短的优先
先声明优先：dependency的排列顺序
```
#聚合和继承
聚合
```
<packaging>pom</packaging>

<modules>
    <module>A相对路径（../hongxing-bge）</module>
    <module>B</module>
    <module>C</module>
</modules>
```

继承
```
并不会在项目中运行，新建一个parent的maven项目（包含公共的jar包）    

<packaging>pom</packaging>

<properties>
    <junit.version>3.8.1</junit.version>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>

<dependencyManagement>
            <dependencies>
                <dependency>
                    <groupId>junit</groupId>
                    <artifactId>junit</artifactId>
                    <version>${junit.version}</version>
                    <scope>test</scope>
                </dependency>
            </dependencies>
    </dependencyManagement>
    
在其他maven项目中可以继承

<parent>
    父pom的坐标
</parent>

<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <!--    此位置的两行可删除  -->
    </dependency>
</dependencies>
```

#使用maven构建web项目
```
New maven project, choose:
    maven-archetype-webapp
index.jsp飘红，原因是没有加入servlet的jar，到官网找到：
    <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.0.1</version>
			<!-- 只在编译和测试时运行 -->
			<scope>provided</scope>
		</dependency>
默认maven项目的src下只有resource要手动添加sourcefolder：/src/main/java可能会提示已存在，此时切换到navigate窗口之间创建java目录即可
```

配置运行环境
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>edu.tongji.webdemo</groupId>
	<artifactId>webdemo</artifactId>
	<packaging>war</packaging>
	<version>0.0.1-SNAPSHOT</version>
	<name>webdemo Maven Webapp</name>
	<url>http://maven.apache.org</url>
	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.10</version>
			<scope>test</scope>
		</dependency>
		<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.0.1</version>
			<!-- 只在编译和测试时运行 -->
			<scope>provided</scope>
		</dependency>
	</dependencies>
	<build>
		<finalName>webdemo</finalName>
		<plugins>
			<plugin>
				<!-- <groupId>org.mortbay.jetty</groupId> <artifactId>jetty-maven-plugin</artifactId> 
					<version>8.1.16.v20140903</version> -->
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<version>2.2</version>

				<!-- 在打包成功后运行jetty:run来运行jetty服务 -->
				<executions>
					<execution>
						<phase>package</phase>
						<goals>
							<goal>run</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>
</project>

jetty maven plugin在maven的官方repo中
tomcat在tomcat官网的Maven Plugin中

pom.xml run as maven build, clean package / jetty:run 
```
