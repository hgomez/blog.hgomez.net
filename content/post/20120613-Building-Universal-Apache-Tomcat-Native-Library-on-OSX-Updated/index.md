+++
title = 'Building Universal Apache Tomcat Native Library on OSX - Updated'
date = 2012-06-13T13:20:23+02:00
draft = false
tags = [ 'OSX', 'TCNative', 'Tomcat' ]
categories = [ 'OS' ]
image = 'tcnativeosx.png'
+++

Updated build process for tomcat-connector, 1.1.24, no more APXS variable or configure parameters required

```
TCN_RELEASE=1.1.24
curl http://mir2.ovh.net/ftp.apache.org/dist//tomcat/tomcat-connectors/native/$TCN_RELEASE/source/tomcat-native-$TCN_RELEASE-src.tar.gz -o tomcat-native-$TCN_RELEASE-src.tar.gz
tar xvzf tomcat-native-$TCN_RELEASE-src.tar.gz
cd tomcat-native-$TCN_RELEASE-src/jni/native

CFLAGS='-arch i386 -arch x86_64' APXSLDFLAGS='-arch i386 -arch x86_64' ./configure --with-apr=/usr --with-ssl=/usr --with-java-home=/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
make clean
make

sudo cp .libs/libtcnative-1.0.1.24.dylib /usr/lib/java
sudo rm -f  /usr/lib/java/libtcnative-1.dylib
sudo ln -s /usr/lib/java/libtcnative-1.0.1.24.dylib /usr/lib/java/libtcnative-1.dylib
```
### A note about Lion

If you get Java on Lion using the java command on terminal or via the Java Developer Package for Mac OS X 10.7, Java headers are not on the usual location and you could find them under **/System/Library/Frameworks/JavaVM.framework/Versions/A/Headers**

You should then update the **configure** command line like this :

```
CFLAGS='-arch i386 -arch x86_64' ./configure --with-apr=/usr --with-ssl=/usr --with-java-home=/System/Library/Frameworks/JavaVM.framework/Versions/A/
```

```
TCN_RELEASE=1.1.24
curl http://mir2.ovh.net/ftp.apache.org/dist//tomcat/tomcat-connectors/native/$TCN_RELEASE/source/tomcat-native-$TCN_RELEASE-src.tar.gz -o tomcat-native-$TCN_RELEASE-src.tar.gz
tar xvzf tomcat-native-$TCN_RELEASE-src.tar.gz
cd tomcat-native-$TCN_RELEASE-src/jni/native

CFLAGS='-arch i386 -arch x86_64' ./configure --with-apr=/usr --with-ssl=/usr --with-java-home=/System/Library/Frameworks/JavaVM.framework/Versions/A/
make clean
make

sudo cp .libs/libtcnative-1.0.dylib /usr/lib/java
sudo rm -f  /usr/lib/java/libtcnative-1.dylib
sudo ln -s /usr/lib/java/libtcnative-1.0.dylib /usr/lib/java/libtcnative-1.dylib
```

