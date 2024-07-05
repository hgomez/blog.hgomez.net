+++
title = 'OpenJDK 7 on OSX and IntelliJ IDEA EAP'
date = 2011-04-16T13:20:23+02:00
draft = false
tags = [ 'IntelliJ', 'OpenJDK', 'OSX'  ]
categories = [ 'OS' ]
image = 'intellijosxopenjdk.png'
+++

During the last weeks, Apple started to contribute it’s Aqua/Cocoa port to OpenJDK 7 on the macosx-port branch. It’s still works it progress but it was nice to see some SWING apps like IntelliJ IDEA works on the preliminary release.

I used IntelliJ IDEA EAP (10.5) and tweaked it’s startup shell.sh to define AWT_TOOLKIT=CToolkit and add -Dswing.defaultlaf=com.apple.laf.AquaLookAndFeel since Aqua Look and Feel is not default for now.

```
export CLASSPATH
LD_LIBRARY_PATH=.:$LD_LIBRARY_PATH export LD_LIBRARY_PATHAWT_TOOLKIT
```
# AWT/Cocoa port for OpenJDK 7 macosx-port

```
export=CToolkit JVM_ARGS=”-Dswing.defaultlaf=com.apple.laf.AquaLookAndFeel $JVM_ARGS”

cd “$IDEA_BIN_HOME” while true ; do $IDEA_JDK/bin/java $JVM_ARGS -Djb.restart.code=88 $IDEA_MAIN_CLASS_NAME $* test $? -ne 88 && break done
```

Then defined OpenJDK 7 from macosx port (available [here](http://openjdk-osx-build.googlecode.com/files/OpenJDK-OSX-1.7-universal-20110416.dmg)) as default JVM and started IntelliJ by calling its shell script :

[bash] export JAVA_HOME=/Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home /Applications/IdeaX-IU-106.396.app/bin/idea.sh [/bash]

And I could see a Swing based application running on OpenJDK 7 and OS/X.

