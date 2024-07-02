+++
title = 'Maven JMeter Plugin'
date = 2010-11-04T13:20:23+02:00
draft = false
tags = [ 'JMeter', 'Maven' ]
+++

While playing with JMeter Maven plugin, I got some problems :

First the official Jakarta plugin is pretty old and no more maintened.

So you should get a new one, from [GoogleCode](http://code.google.com/p/jmeter-maven-plugin/) This project moved to [GitHub](https://github.com/Ronnie76er/jmeter-maven-plugin)

Great project but it miss 2 dependencies, **commons-logging** and **soap** Without **commons-logging** your tests may fail and you could see : 

```
Error in NonGUIDriver java.lang.NullPointerException [INFO] ———————————————————————— [ERROR] BUILD FAILURE [INFO] ———————————————————————— [INFO] There were test errors
```

The fix is easy, get the plugin project and add the following dependencies in it :

```
<dependency>
	<groupId>soap</groupId>
	<artifactId>soap</artifactId>
	<version>2.3.1</version>
</dependency>

<dependency>
	<groupId>commons-logging</groupId>
	<artifactId>commons-logging</artifactId>
	<version>1.1.1</version>
</dependency>
```


Just rebuild and install (or deploy), this updated plugin, may be changing it’s version from **1.0** to **1.1-SNAPSHOT** to avoid conflict with current one.

Full pom.xml is here :

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>org.apache.jmeter</groupId>
	<artifactId>maven-jmeter-plugin</artifactId>
	<packaging>maven-plugin</packaging>
	<version>1.0</version>
	<name>Maven JMeter Plugin</name>
	<url>http://jakarta.apache.org/jmeter</url>

	<dependencies>
		<dependency>
			<groupId>ant</groupId>
			<artifactId>ant</artifactId>
			<version>1.6</version>
		</dependency>
		<dependency>
			<groupId>commons-io</groupId>
			<artifactId>commons-io</artifactId>
			<version>1.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.maven</groupId>
			<artifactId>maven-plugin-api</artifactId>
			<version>2.0</version>
		</dependency>
		<dependency>
			<groupId>org.apache.maven</groupId>
			<artifactId>maven-project</artifactId>
			<version>2.0.9</version>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.apache.jmeter</groupId>
			<artifactId>jmeter</artifactId>
			<version>2.3</version>
		</dependency>

		<dependency>
			<groupId>soap</groupId>
			<artifactId>soap</artifactId>
			<version>2.3.1</version>
		</dependency>

		<dependency>
			<groupId>commons-logging</groupId>
			<artifactId>commons-logging</artifactId>
			<version>1.1.1</version>
		</dependency>

		<dependency>
			<groupId>org.apache.jmeter</groupId>
			<artifactId>jmeter</artifactId>
			<version>2.3</version>
		</dependency>
		<dependency>
			<groupId>org.apache.maven.wagon</groupId>
			<artifactId>wagon-webdav</artifactId>
			<version>1.0-beta-2</version>
		</dependency>
	</dependencies>
	<distributionManagement>
		<repository>
			<id>central</id>
			<name>Gestalt Central Repo</name>
			<url>dav://artifactory.int.gestalt-llc.com:8080/archiva/repository/repo</url>
		</repository>

	</distributionManagement>

	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>2.0.2</version>
				<configuration>
					<source>1.5</source>
					<target>1.5</target>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-surefire-plugin</artifactId>
				<version>2.3</version>
			</plugin>
		</plugins>
	</build>
</project>
```

I hope this plugin will soon include these fixes and update JMeter to its latest version, 2.4 :)