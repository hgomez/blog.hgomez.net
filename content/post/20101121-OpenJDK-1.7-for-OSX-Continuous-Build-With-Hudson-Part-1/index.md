+++
title = 'OpenJDK 1.7 for OSX Continuous Build With Hudson - Part 1'
date = 2010-11-21T13:20:23+02:00
draft = false
tags = [ 'Hudson', 'OpenJDK', 'OSX' ]
categories = [ 'OS' ]
image = 'openjdkosxhudson.png'
+++
## Pre-requisite

- an OS/X box, under Snow Leopard, 32 and 64bits mode should works
- [XCode](https://web.archive.org/web/20120205101528/http://developer.apple.com/technologies/tools/xcode.html)
- Mercurial with hgforest extension (see my previous article on [Mercurial and hgforest](https://web.archive.org/web/20120205101528/http://blog.hgomez.net/?p=649))
- Hudson with its [Mercurial Plugin](https://web.archive.org/web/20120205101528/http://wiki.hudson-ci.org/display/HUDSON/Mercurial+Plugin)

## Hudson jobs

I created **free-style software project** jobs, one for building 32 bits JVM, **openjdk-1.7-i586**, the other to build 64 bits JVM, **openjdk-1.7-x86_64**.

Each one will use self sufficient script, each script will :

- download soylatte JVMs (i386/amd64) under $HUDSON_HOME/DROP_DIR (so it could be reused for future builds).
- jaxp, jaf and jaxws2 since these are not available from the url defined in ant build scripts. Also download under $HUDSON_HOME/DROP_DIR.
- patch project so it will build under 32 and 64bits OS/X, and will be able to build 32bits JVM on 64bits OS/X (patches commited to OpenJDK Bugzilla [#100155](https://web.archive.org/web/20120205101528/https://bugs.openjdk.java.net/show_bug.cgi?id=100155))
