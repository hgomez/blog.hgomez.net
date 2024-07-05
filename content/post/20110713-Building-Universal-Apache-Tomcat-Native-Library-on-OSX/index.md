+++
title = 'Building Universal Apache Tomcat Native Library on OSX'
date = 2011-07-13T13:20:23+02:00
draft = false
tags = [ 'TCNative', 'OSX', 'Tomcat'  ]
categories = [ 'OS' ]
image = 'showtomcat.png'
+++

I recently notice that my Apache Tomcat running on OS/X 10.6.8 couldn’t use Apache Tomcat Native Library.

```
INFO: The APR based Apache Tomcat Native library which allows optimal
performance in production environments was not found on the
java.library.path:
.:/Library/Java/Extensions:/System/Library/Java/Extensions:/usr/lib/java
Jul 13, 2011 11:02:30 AM org.apache.coyote.http11.Http11Protocol init
```

After digging around and with the help of ASFer Mladen Turk, I figure my previous build was stick to 64bits mode only and I switched my JVM to 32bits mode using -d32.

The fix was then easy, just had to rebuild tomcat-native and asking OS/X gcc to produce both 32/64 bits model library using the following CLFAGS/APXSLDFLAGS.

```
CFLAGS='-arch i386 -arch x86_64' APXSLDFLAGS='-arch i386-arch x86_64'
```

Here is a small script I’m using now to produce Apache Tomcat Native Library on OS/X.

```
curl http://mir2.ovh.net/ftp.apache.org/dist/tomcat/tomcat-connectors/native/1.1.23/source/tomcat-native-1.1.23-src.tar.gz -o tomcat-native-1.1.23-src.tar.gz
tar xvzf tomcat-native-1.1.23-src.tar.gz
cd tomcat-native-1.1.23-src/jni/native

CFLAGS='-arch i386 -arch x86_64' APXSLDFLAGS='-arch i386 -arch x86_64' ./configure --with-apr=/usr --with-ssl=/usr --with-java-home=/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home --with-apxs=/usr/sbin/apxs
make clean
make

sudo cp .libs/libtcnative-1.0.1.23.dylib /usr/lib/java
sudo rm -f  /usr/lib/java/libtcnative-1.dylib
sudo ln -s /usr/lib/java/libtcnative-1.0.1.23.dylib /usr/lib/java/libtcnative-1.dylib
```

### A note about Lion

If you get Java on Lion using the java command on terminal or via the Java Developer Package for Mac OS X 10.7, Java headers are not on the usual location and you could find them under **/System/Library/Frameworks/JavaVM.framework/Versions/A/Headers**

You should then update the **configure** command line like this :

```
CFLAGS='-arch i386 -arch x86_64' APXSLDFLAGS='-arch i386 -arch x86_64' ./configure --with-apr=/usr --with-apxs=/usr/sbin/apxs --with-ssl=/usr --with-java-home=/System/Library/Frameworks/JavaVM.framework/Versions/A/
```

Lion came with Xcode 4.1 and there is also an impact on linker side, libtcnative is now produced as **libtcnative-1.0.dylib**

Commands became so :

```
curl http://mir2.ovh.net/ftp.apache.org/dist//tomcat/tomcat-connectors/native/1.1.23/source/tomcat-native-1.1.23-src.tar.gz -o tomcat-native-1.1.23-src.tar.gz
tar xvzf tomcat-native-1.1.23-src.tar.gz
cd tomcat-native-1.1.23-src/jni/native

CFLAGS='-arch i386 -arch x86_64' APXSLDFLAGS='-arch i386 -arch x86_64' ./configure --with-apr=/usr --with-apxs=/usr/sbin/apxs --with-ssl=/usr --with-java-home=/System/Library/Frameworks/JavaVM.framework/Versions/A/
make clean
make

sudo cp .libs/libtcnative-1.0.dylib /usr/lib/java
sudo rm -f  /usr/lib/java/libtcnative-1.dylib
sudo ln -s /usr/lib/java/libtcnative-1.0.dylib /usr/lib/java/libtcnative-1.dylib
```