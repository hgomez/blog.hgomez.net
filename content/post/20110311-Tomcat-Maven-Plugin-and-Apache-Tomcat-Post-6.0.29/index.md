+++
title = 'Tomcat Maven Plugin and Apache Tomcat Post 6.0.29'
date = 2011-03-11T13:20:23+02:00
draft = false
tags = [ 'Maven', 'Tomcat' ]
categories = [ 'Software' ]
image = 'tomcatmaven.png'
+++

If you’re using the [Tomcat Maven Plugin](http://mojo.codehaus.org/tomcat-maven-plugin/) and want to use post 6.0.29  Apache Tomcat, ie latest 6.0.32, you should update your pom to handle a change in artifact.

Up to 6.0.29, Eclipse JDT compiler was bundled as jasper-jdt :

```
          <groupId>org.apache.tomcat</groupId>
          <artifactId>jasper-jdt</artifactId>
          <version>6.0.29</version>
        </dependency>
```

With [6.0.30](http://tomcat.apache.org/tomcat-6.0-doc/changelog.html), Apache Tomcat team started to bundle Eclipse JDT directly:

[xml] org.eclipse.jdt.core.compiler ecj 3.5.1 [/xml]

As consequence, org.apache.tomcat/jasper-jdt artifact didn’t exist anymore after release 6.0.29.

[caption id=”attachment_742” align=”alignnone” width=”743” caption=”Jasper JDT up to 6.0.29”][![Jasper JDT up to 6.0.29](http://blog.hgomez.net/wp-content/uploads/2011/03/jasper-jdt.png)](http://blog.hgomez.net/wp-content/uploads/2011/03/jasper-jdt.png)[/caption]

For Tomcat Maven Plugin, you should update the [suggested pom](http://mojo.codehaus.org/tomcat-maven-plugin/examples/adjust-embedded-tomcat-version.html) like this :

```
<build>
    <pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>tomcat-maven-plugin</artifactId>
                <version>1.2-SNAPSHOT</version>
                <dependencies>
                    <dependency>
                        <groupId>org.apache.tomcat</groupId>
                        <artifactId>catalina</artifactId>
                        <version>${tomcat.version}</version>
                    </dependency>
                    <dependency>
                        <groupId>org.apache.tomcat</groupId>
                        <artifactId>catalina-ha</artifactId>
                        <version>${tomcat.version}</version>
                        <scope>runtime</scope>
                    </dependency>
                    <dependency>
                        <groupId>org.apache.tomcat</groupId>
                        <artifactId>tribes</artifactId>
                        <version>${tomcat.version}</version>
                        <scope>runtime</scope>
                    </dependency>
                    <dependency>
                        <groupId>org.apache.tomcat</groupId>
                        <artifactId>el-api</artifactId>
                        <version>${tomcat.version}</version>
                        <scope>runtime</scope>
                    </dependency>
                    <dependency>
                        <groupId>org.apache.tomcat</groupId>
                        <artifactId>jasper</artifactId>
                        <version>${tomcat.version}</version>
                        <scope>runtime</scope>
                    </dependency>
                    <dependency>
                        <groupId>org.apache.tomcat</groupId>
                        <artifactId>jasper-el</artifactId>
                        <version>${tomcat.version}</version>
                        <scope>runtime</scope>
                    </dependency>
                    <dependency>
                        <groupId>org.eclipse.jdt.core.compiler</groupId>
                        <artifactId>ecj</artifactId>
                        <version>3.5.1</version>
                        <scope>runtime</scope>
                    </dependency>
                    <dependency>
                        <groupId>org.apache.tomcat</groupId>
                        <artifactId>jsp-api</artifactId>
                        <version>${tomcat.version}</version>
                        <scope>runtime</scope>
                    </dependency>
                    <dependency>
                        <groupId>org.apache.tomcat</groupId>
                        <artifactId>servlet-api</artifactId>
                        <version>${tomcat.version}</version>
                        <scope>runtime</scope>
                    </dependency>
                    <dependency>
                        <groupId>org.apache.tomcat</groupId>
                        <artifactId>coyote</artifactId>
                        <version>${tomcat.version}</version>
                        <scope>runtime</scope>
                    </dependency>
                    <dependency>
                        <groupId>org.apache.tomcat</groupId>
                        <artifactId>dbcp</artifactId>
                        <version>${tomcat.version}</version>
                        <scope>runtime</scope>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </pluginManagement>
```

### Notice: An updated version of Tomcat Maven Plugin 1.2-SNAPSHOT has been released, this hack is no more necessary

