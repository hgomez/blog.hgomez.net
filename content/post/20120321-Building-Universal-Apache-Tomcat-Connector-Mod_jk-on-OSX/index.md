+++
title = 'Building Universal Apache Tomcat Connector (Mod_jk) on OSX'
date = 2012-03-21T13:20:23+02:00
draft = false
tags = [ 'mod_jk', 'OSX', 'Tomcat' ]
categories = [ 'OS' ]
image = 'osxtomcatapxs.png'
+++

Build Universal Apache Tomcat Connector (mod_jk) for OSX follow tricks used for Apache Tomcat Native Library.

```
CFLAGS='-arch i386 -arch x86_64' APXSLDFLAGS='-arch i386-arch x86_64'
```

Here is a small script to do it :

```
#!/bin/sh
#

JK_VERSION=1.2.37

curl http://mir2.ovh.net/ftp.apache.org/dist/tomcat/tomcat-connectors/jk/tomcat-connectors-${JK_VERSION}-src.tar.gz -o tomcat-connectors-${JK_VERSION}-src.tar.gz
tar xvzf tomcat-connectors-${JK_VERSION}-src.tar.gz
cd tomcat-connectors-${JK_VERSION}-src/native

./configure --with-apxs=/usr/sbin/apxs CFLAGS='-arch i386 -arch x86_64' APXSLDFLAGS='-arch i386-arch x86_64'
make clean
make
```

Installation is pretty simple :

```
sudo cp apache-2.0/.libs/mod_jk.so /usr/libexec/apache2/
```

You could then restart your Apache HTTPd server to get new mod_jk used :

```
sudo /usr/sbin/apachectl restart
```