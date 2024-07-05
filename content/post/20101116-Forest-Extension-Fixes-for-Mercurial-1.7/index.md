+++
title = 'Forest Extension Fixes for Mercurial 1.7'
date = 2010-11-16T13:20:23+02:00
draft = false
tags = [ 'Mercurial' ]
categories = [ 'Software' ]
image = 'mercurial.png'
+++

I’m using Mercurial 1.7 from MacPorts to sync with OpenJDK sources. This operation is usually done with **hg fclone**

[bash] hg fclone http://hg.openjdk.java.net/bsd-port/bsd-port [/bash]

**fclone** came from **forest**

Mercurial 1.7 changes some API and call to **do_read** should be changed to **_call**.

Hopefully, I found a maintained **Forest** for Mercurial by GXTI and they now handle correctly pre/post Mercurial 1.6 or 1.7.

To install this updated extension.

```
hg clone https://bitbucket.org/gxti/hgforest
```

Make sure this forest.py is included in your **~/.hgrc**

```
hgext.forest=/Users/henri/hgforest/forest.py
```

