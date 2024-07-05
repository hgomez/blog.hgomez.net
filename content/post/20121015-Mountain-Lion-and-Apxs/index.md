+++
title = 'Mountain Lion and Apxs'
date = 2012-10-15T13:20:23+02:00
draft = false
tags = [ 'APXS', 'OSX' ]
categories = [ 'OS' ]
image = 'mountainlion-apxs.png'
+++

Mountain Lion came with a version of apxs where C compiler and pre-processor are defined to a location not in phase with XCode 4.5.

If you try to build any apxs related modules or Tomcat Native Library, it will fail like this :

```
checking build system type... x86_64-apple-darwin12.2.0
checking host system type... x86_64-apple-darwin12.2.0
checking target system type... x86_64-apple-darwin12.2.0
checking for a BSD-compatible install... /usr/bin/install -c
checking for working mkdir -p... yes
Tomcat Native Version: 1.1.24
checking for chosen layout... tcnative
checking for APR... yes
  setting CC to "/Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain/usr/bin/cc"
  setting CPP to "/Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain/usr/bin/cc -E"
checking for JDK location (please wait)... /System/Library/Frameworks/JavaVM.framework/Versions/A/
checking Java platform... checking Java platform...
checking for sablevm... NONE
  adding "-I/System/Library/Frameworks/JavaVM.framework/Versions/A//Headers" to TCNATIVE_PRIV_INCLUDES
checking os_type directory... jni_md.h found in /System/Library/Frameworks/JavaVM.framework/Versions/A//Headers
checking for gcc... /Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain/usr/bin/cc
checking whether the C compiler works... no
configure: error: in `/Users/henri/tomcat-native-1.1.24-src/jni/native':
configure: error: C compiler cannot create executables
See `config.log' for more details
```

apxs get its configurations from **/usr/share/httpd/build/config_vars.mk** who reference **/Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain**.

```
CC = /Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain/usr/bin/cc
CPP = /Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain/usr/bin/cc -E
```

If you installed XCode to standard location, you’ll find toolchain at **/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain**.

So until Apple fix apxs, you should create a symlink between real and expected location :

```
sudo ln -s /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain /Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain
```

Configure will now works as expected :

```
mbp-rico:native henri$ CFLAGS='-arch i386 -arch x86_64' ./configure --with-apr=/usr --with-ssl=/usr --with-java-home=/System/Library/Frameworks/JavaVM.framework/Versions/A/
checking build system type... x86_64-apple-darwin12.2.0
checking host system type... x86_64-apple-darwin12.2.0
checking target system type... x86_64-apple-darwin12.2.0
checking for a BSD-compatible install... /usr/bin/install -c
checking for working mkdir -p... yes
Tomcat Native Version: 1.1.24
checking for chosen layout... tcnative
checking for APR... yes
  setting CC to "/Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain/usr/bin/cc"
  setting CPP to "/Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain/usr/bin/cc -E"
checking for JDK location (please wait)... /System/Library/Frameworks/JavaVM.framework/Versions/A/
checking Java platform... checking Java platform...
checking for sablevm... NONE
  adding "-I/System/Library/Frameworks/JavaVM.framework/Versions/A//Headers" to TCNATIVE_PRIV_INCLUDES
checking os_type directory... jni_md.h found in /System/Library/Frameworks/JavaVM.framework/Versions/A//Headers
checking for gcc... /Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain/usr/bin/cc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables...
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether /Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain/usr/bin/cc accepts -g... yes
checking for /Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.8.xctoolchain/usr/bin/cc option to accept ISO C89... none needed
checking for OpenSSL library... using openssl from /usr/lib and /usr/include
checking OpenSSL library version... ok
checking for OpenSSL DSA support... yes
  setting TCNATIVE_LDFLAGS to "-lssl -lcrypto"
  adding "-DHAVE_OPENSSL" to CFLAGS
  setting TCNATIVE_LIBS to ""
  setting TCNATIVE_LIBS to " -L/usr/lib -R/usr/lib -lapr-1 -lpthread"
configure: creating ./config.status
config.status: creating tcnative.pc
config.status: creating Makefile
config.status: executing default commands
```

Thanks [stack-exchange](http://apple.stackexchange.com/questions/58186/how-to-compile-mod-wsgi-mod-fastcgi-etc-on-mountain-lion-by-fixing-apxserror) for covering this issue