### Spring Boot父级依赖spring-boot-starter-parent

***

我们在创建Maven构建的Spring Boot项目时，会自动生成一个Pom.xml的文件

，其中有一段代码如下：

```java
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.0.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
</parent>
```

它用来提供Maven的的默认依赖，常用的包依赖就可以省略包版本(version)信息,使用该设置，还可以通过自己的属性来覆盖各个依赖项。

我们可以在本地仓库打开它的源码来一探究竟，在我这台电脑上的路径是C:\Users\k8402\.m2\repository\org\springframework\boot\spring-boot-starter-parent\2.2.0.RELEASE.spring-boot-starter-parent-2.2.0.RELEASE.pom

```java
<?xml version="1.0" encoding="utf-8"?><project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.2.0.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
  </parent>
  <artifactId>spring-boot-starter-parent</artifactId>
  <packaging>pom</packaging>
  <name>Spring Boot Starter Parent</name>
  <description>Parent pom providing dependency and plugin management for applications
		built with Maven</description>
  <url>https://projects.spring.io/spring-boot/#/spring-boot-starter-parent</url>
  <properties>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <java.version>1.8</java.version>
    <resource.delimiter>@</resource.delimiter>
    <maven.compiler.source>${java.version}</maven.compiler.source>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.target>${java.version}</maven.compiler.target>
  </properties>
  <build>
    <resources>
      <resource>
        <filtering>true</filtering>
        <directory>${basedir}/src/main/resources</directory>
        <includes>
          <include>**/application*.yml</include>
          <include>**/application*.yaml</include>
          <include>**/application*.properties</include>
        </includes>
      </resource>
      <resource>
        <directory>${basedir}/src/main/resources</directory>
        <excludes>
          <exclude>**/application*.yml</exclude>
          <exclude>**/application*.yaml</exclude>
          <exclude>**/application*.properties</exclude>
        </excludes>
      </resource>
    </resources>
    <pluginManagement>
      <plugins>
        <plugin>
          <groupId>org.jetbrains.kotlin</groupId>
          <artifactId>kotlin-maven-plugin</artifactId>
          <version>${kotlin.version}</version>
          <executions>
            <execution>
              <id>compile</id>
              <phase>compile</phase>
              <goals>
                <goal>compile</goal>
              </goals>
            </execution>
            <execution>
              <id>test-compile</id>
              <phase>test-compile</phase>
              <goals>
                <goal>test-compile</goal>
              </goals>
            </execution>
          </executions>
          <configuration>
            <jvmTarget>${java.version}</jvmTarget>
            <javaParameters>true</javaParameters>
          </configuration>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <configuration>
            <parameters>true</parameters>
          </configuration>
        </plugin>
        <plugin>
          <artifactId>maven-failsafe-plugin</artifactId>
          <executions>
            <execution>
              <goals>
                <goal>integration-test</goal>
                <goal>verify</goal>
              </goals>
            </execution>
          </executions>
          <configuration>
            <classesDirectory>${project.build.outputDirectory}</classesDirectory>
          </configuration>
        </plugin>
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <configuration>
            <archive>
              <manifest>
                <mainClass>${start-class}</mainClass>
                <addDefaultImplementationEntries>true</addDefaultImplementationEntries>
              </manifest>
            </archive>
          </configuration>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <configuration>
            <archive>
              <manifest>
                <mainClass>${start-class}</mainClass>
                <addDefaultImplementationEntries>true</addDefaultImplementationEntries>
              </manifest>
            </archive>
          </configuration>
        </plugin>
        <plugin>
          <groupId>org.codehaus.mojo</groupId>
          <artifactId>exec-maven-plugin</artifactId>
          <configuration>
            <mainClass>${start-class}</mainClass>
          </configuration>
        </plugin>
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <configuration>
            <delimiters>
              <delimiter>${resource.delimiter}</delimiter>
            </delimiters>
            <useDefaultDelimiters>false</useDefaultDelimiters>
          </configuration>
        </plugin>
        <plugin>
          <groupId>pl.project13.maven</groupId>
          <artifactId>git-commit-id-plugin</artifactId>
          <executions>
            <execution>
              <goals>
                <goal>revision</goal>
              </goals>
            </execution>
          </executions>
          <configuration>
            <verbose>true</verbose>
            <dateFormat>yyyy-MM-dd'T'HH:mm:ssZ</dateFormat>
            <generateGitPropertiesFile>true</generateGitPropertiesFile>
            <generateGitPropertiesFilename>${project.build.outputDirectory}/git.properties</generateGitPropertiesFilename>
          </configuration>
        </plugin>
        <plugin>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-maven-plugin</artifactId>
          <executions>
            <execution>
              <id>repackage</id>
              <goals>
                <goal>repackage</goal>
              </goals>
            </execution>
          </executions>
          <configuration>
            <mainClass>${start-class}</mainClass>
          </configuration>
        </plugin>
        <plugin>
          <artifactId>maven-shade-plugin</artifactId>
          <executions>
            <execution>
              <phase>package</phase>
              <goals>
                <goal>shade</goal>
              </goals>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
                    <resource>META-INF/spring.handlers</resource>
                  </transformer>
                  <transformer implementation="org.springframework.boot.maven.PropertiesMergingResourceTransformer">
                    <resource>META-INF/spring.factories</resource>
                  </transformer>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
                    <resource>META-INF/spring.schemas</resource>
                  </transformer>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer"/>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                    <mainClass>${start-class}</mainClass>
                  </transformer>
                </transformers>
              </configuration>
            </execution>
          </executions>
          <dependencies>
            <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-maven-plugin</artifactId>
              <version>2.2.0.RELEASE</version>
            </dependency>
          </dependencies>
          <configuration>
            <keepDependenciesWithProvidedScope>true</keepDependenciesWithProvidedScope>
            <createDependencyReducedPom>true</createDependencyReducedPom>
            <filters>
              <filter>
                <artifact>*:*</artifact>
                <excludes>
                  <exclude>META-INF/*.SF</exclude>
                  <exclude>META-INF/*.DSA</exclude>
                  <exclude>META-INF/*.RSA</exclude>
                </excludes>
              </filter>
            </filters>
          </configuration>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```

通过上面的代码可以看出，它继承自**spring-boot-dependencies**，在这里有基本的依赖信息，以及项目的编码格式和java版本等，最后再来看看源码中指定的spring-boot-dependencies的信息(该文件中代码实在过多，只举例说明)：

```java
<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
        <version>2.2.0.RELEASE</version>
</dependency>
```

在它继承的**spring-boot-dependencies**里面已经写好了版本，所以我们不需要再声明了。

当然，有可能你并不想从spring-boot-starter-parent来继承，可能你需要使用自己公司所提供的父级，则你可以通过**scope=import**来保持依赖项：

```java
<dependencyManagement>
		<dependencies>
		<dependency>
			<!-- Import dependency management from Spring Boot -->
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-dependencies</artifactId>
			<version>2.0.2.RELEASE</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
	</dependencies>
</dependencyManagement>
```

但这么做之后，就不允许使用属性来覆盖依赖项，有一种解决方法，在Pom.xml中添加如下信息：

```java
<dependencyManagement>
	<dependencies>
		<!-- Override Spring Data release train provided by Spring Boot -->
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-releasetrain</artifactId>
			<version>Fowler-SR2</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
    
		<!--这是添加项-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-dependencies</artifactId>
			<version>2.0.2.RELEASE</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
    
	</dependencies>
</dependencyManagement>
```


