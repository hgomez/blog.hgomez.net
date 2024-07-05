+++
title = 'Uninstalling MacFuse on Lion'
date = 2012-01-28T13:20:23+02:00
draft = false
tags = [ 'MacFuse', 'OSX' ]
categories = [ 'OS' ]
image = 'fuselion.png'
+++

If you installed MacFuse on Lion (10.7) and tried to uninstall you may encountered the following error :

```
sudo /Library/Filesystems/fusefs.fs/Support/uninstall-macfuse-core.sh
MacFUSE Uninstaller: Can not find the Archive.bom for MacFUSE Core package.
```

Uninstaller didn’t check for Lion (uname -r reporting 11.x). So fix is easy, just edit uninstaller script **/Library/Filesystems/fusefs.fs/Support/uninstall-macfuse-core.sh** and add 11_) in case next to 10_)

```
...

OS_RELEASE=`/usr/bin/uname -r`
case "$OS_RELEASE" in
  8*)
    log "Incorrect uninstall. Use the Tiger version please."
    exit 1
    ;;
  9*)
    PACKAGE_RECEIPT="$INSTALL_VOLUME/Library/Receipts/MacFUSE Core.pkg"
    OUTER_PACKAGE_RECEIPT="$INSTALL_VOLUME/Library/Receipts/MacFUSE.pkg"
    BOMFILE="$PACKAGE_RECEIPT/Contents/Archive.bom"
    ;;
  10*|11*)
     PACKAGE_RECEIPT=""
     BOMFILE="$INSTALL_VOLUME/var/db/receipts/com.google.macfuse.core.bom"
     ;;
esac
```