+++
title = 'Unifying Handling of Tomcat and HTTP Error Pages'
date = 2010-10-24T13:20:23+02:00
draft = false
tags = [ 'Httpd', 'Tomcat' ]
categories = [ 'Run' ]
image = 'httpdtomcaterrors.png'
+++

In configuration with a front Apache HTTPd 2.2.x server and backend Tomcat servers, you may have defined customs error page on the HTTPd configuration using [ErrorDocument](http://httpd.apache.org/docs/2.2/mod/core.html#errordocument) directive.

```
ErrorDocument 401 /errors/err-401.html ErrorDocument 403 /errors/err-403.html ErrorDocument 404 /errors/err-404.html ErrorDocument 500 /errors/err-500.html ```
```

It works well for resources handled by HTTPd but errors for contents served by Tomcat are still handled by Tomcat error mecanism.

Imagine a web application myapp, served by a Tomcat behind HTTPd, you could have the following setup.

```
JkMount /myapp/* worker1
```

If someone hit an inexisting page, ie /myapp/myfaultypage, you’ll get back the 404 error from Tomcat but not the one defined in HTTPd.

Since version 1.2.27, mod_jk could handle such situation via [uriworkermap](http://tomcat.apache.org/connectors-doc/reference/uriworkermap.html) **use_server_errors** directive.

But if you don’t want to change your current setup, ie using **JkMount**, you could use the following :

```
JkMount /myapp/* worker1;use_server_errors=400
```


If Tomcat allready handle Page Not Found, 404, and you only wan’t to see Apache HTTPd handling error greater than or egal to 500, just use :

```
JkMount /myapp/* worker1;use_server_errors=500
```

Thanks to Mladen for the trick :)