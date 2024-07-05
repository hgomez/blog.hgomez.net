+++
title = 'OpenJDK 1.7 for OSX Continuous Build With Hudson - Part 2'
date = 2010-11-25T13:20:23+02:00
draft = false
tags = [ 'Hudson', 'OpenJDK', 'OSX' ]
categories = [ 'OS' ]
image = 'openjdkosxhudson.png'
+++

[First episode](http://blog.hgomez.net/?p=670) of OpenJDK 1.7 for OS/X covered build of 32 and 64 bits VM via Hudson.

We ended with two VMs, 32bits was under build/bsd-i586/j2sdk-image and 64bits in build/bsd-amd64/j2sdk-image And here appears a new OS/X jedi, [Gildas](http://www.hikage.be/), providing .PKG and .DMG scripting.

This episode will describe how packaging, PKG and DMG was done.

## OS/X Package .PKG

First we need to transform j2sdk-image folder into .PKG

We used OS/X **packagemaker**, provided by **XCode** :

```
/Developer/usr/bin/packagemaker \
--title "Open JDK 7 (32bits) for OS X Installer" \
--version 1.0 \
--filter "\.DS_Store" \
--root-volume-only \
--domain system \
--verbose \
--no-relocate \
-l "/Library/Java/JavaVirtualMachines/openjdk-1.7-i586" \
--target 10.5 \
--id net.openjdk.java.i586.pkg \
--root ${SOURCE_DIR} \
--out ${BUILD_DIR}/openjdk-1.7-i586.pkg \
-v
```

## From .PKG to .DMG

Next step is to transform the **.PKG** into a mountable image **.DMG**. Here we used hdiutil :

```
hdiutil create -srcfolder ${BUILD_DIR}/openjdk-1.7-i586.pkg -volname ‘Open JDK 7 (32bits)’ -fs HFS+ -fsargs ‘-c c=64,a=16,e=16’ -format UDRW ${BUILD_DIR}/openjdk-1.7-i586-tmp.dmg hdiutil convert ${BUILD_DIR}/openjdk-1.7-i586-tmp.dmg -format UDZO -imagekey zlib-level=9 -o ${BUILD_DIR}/OpenJDK-1.7-i586.dmg
```

## Scripts for 32 and 64bits VM

The following scripts could be added at the end of the Hudson script zone, or called in a second pass.

### 32bits

```
# !/bin/bash

SOURCE_DIR=`pwd`/build/bsd-i586/j2sdk-image BUILD_DIR=`pwd`/java-osx DMG_MOUNT_DIR=$BUILD_DIR/mount

mkdir -p ${BUILD_DIR}

if [ -x build/bsd-i586/j2sdk-image/bin/java ]; then

rm -f ${BUILD_DIR}/OpenJDK-1.7-i586.dmg ${BUILD_DIR}/openjdk-1.7-i586.pkg

/Developer/usr/bin/packagemaker \
--title "Open JDK 7 (32bits) for OS X Installer" \
--version 1.0 \
--filter "\.DS_Store" \
--root-volume-only \
--domain system \
--verbose \
--no-relocate \
-l "/Library/Java/JavaVirtualMachines/openjdk-1.7-i586" \
--target 10.5 \
--id net.openjdk.java.i586.pkg \
--root ${SOURCE_DIR} \
--out ${BUILD_DIR}/openjdk-1.7-i586.pkg \
-v

hdiutil create -srcfolder ${BUILD_DIR}/openjdk-1.7-i586.pkg -volname ‘Open JDK 7 (32bits)’ -fs HFS+ -fsargs ‘-c c=64,a=16,e=16’ -format UDRW ${BUILD_DIR}/openjdk-1.7-i586-tmp.dmg hdiutil convert ${BUILD_DIR}/openjdk-1.7-i586-tmp.dmg -format UDZO -imagekey zlib-level=9 -o ${BUILD_DIR}/OpenJDK-1.7-i586.dmg

rm -f ${BUILD_DIR}/openjdk-1.7-i586-tmp.dmg

else

echo “no valid exec under build/bsd-i586/j2sdk-image/bin/java, packaging skipped”

fi [/bash]
```

### 64bits

```
# !/bin/bash

SOURCE_DIR=`pwd`/build/bsd-amd64/j2sdk-image BUILD_DIR=`pwd`/java-osx DMG_MOUNT_DIR=$BUILD_DIR/mount

mkdir -p ${BUILD_DIR}

if [ -x build/bsd-amd64/j2sdk-image/bin/java ]; then

rm -f ${BUILD_DIR}/OpenJDK-1.7-x86_64.dmg ${BUILD_DIR}/openjdk-1.7-x86_64.pkg

/Developer/usr/bin/packagemaker \
--title "Open JDK 7 (64bits) for OS X Installer" \
--version 1.0 \
--filter "\.DS_Store" \
--root-volume-only \
--domain system \
--verbose \
--no-relocate \
-l "/Library/Java/JavaVirtualMachines/openjdk-1.7-x86_64" \
--target 10.5 \
--id net.openjdk.java.x86_64.pkg \
--root ${SOURCE_DIR} \
--out ${BUILD_DIR}/openjdk-1.7-x86_64.pkg \
-v

hdiutil create -srcfolder ${BUILD_DIR}/openjdk-1.7-x86_64.pkg -volname ‘Open JDK 7 (64bits)’ -fs HFS+ -fsargs ‘-c c=64,a=16,e=16’ -format UDRW ${BUILD_DIR}/openjdk-1.7-x86_64-tmp.dmg hdiutil convert ${BUILD_DIR}/openjdk-1.7-x86_64-tmp.dmg -format UDZO -imagekey zlib-level=9 -o ${BUILD_DIR}/OpenJDK-1.7-x86_64.dmg

rm -f ${BUILD_DIR}/openjdk-1.7-x86_64-tmp.dmg

else

echo “no valid exec under build/bsd-amd64/j2sdk-image/bin/java, packaging skipped”

fi
```

## Next Step

Gildas and I, will continue to improve these basic .PKG/.DMG scripts and OS/X gurus advices are more than welcome.

Next steps is to find storage on the net so we could provide DMG regularly. Even better, an OS/X box (under SnowLeopard) available on the net, will help us provide continuous DMG.

Plus on est de fous, ….
