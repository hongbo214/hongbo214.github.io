## 如何将jarinstall 到本地仓库
### 使用maven命令安装jar包到本地仓库
mvn install:install-file
 -Dfile=jar包文件全路径 
    -DgroupId=**  
    -DartifactId=**   
    -Dversion=**   
    -Dpackaging=jar   
    -DgeneratePom=true
如果公司有本地仓库，则使用这种方式安装，可以确保依赖统一，方便使用和管理

### 使用配置方式引入
```xml
<dependency>
    <groupId>com.sample</groupId>
    <artifactId>sample</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/src/main/resources/Name_Your_JAR.jar</systemPath>
</dependency>
```
scope为system作用域，会导致依赖传递中断，如果使用此种方式打包，需要被其它项目依赖，则此方式不适用。

### 使用配置方式自动执行打包到本地仓库
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-install-plugin</artifactId>
    <version>2.5.2</version>
    <executions>
        <execution>
            <phase>initialize</phase>
            <goals>
                <goal>install-file</goal>
            </goals>
            <configuration>
                <file>jar包全路径</file>
                <groupId>**</groupId>
                <artifactId>**</artifactId>
                <version>**</version>
                <packaging>jar</packaging>
            </configuration>
        </execution>
    </executions>
</plugin>
```
采用这种方式您可以使用在基础编译包中，然后再其它任何以此为依赖的maven项目中都可以使用。但是如果您需要在同一个pom文件中使用execution节点打包的jar文件依赖，则不可以，您需要将此安装前置。
