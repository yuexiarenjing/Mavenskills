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
mvn -v          #show version details
mvn compile     #compile java project and generate target directory contains classes file
mvn test        #run debug test
mvn package     #generate jar

mvn clean       #delete target directory
mvn install     #install project into maven repository make other project can use it


```
