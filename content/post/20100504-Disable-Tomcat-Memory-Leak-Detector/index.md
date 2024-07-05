+++
title = 'Disable Tomcat Memory Leak Detector'
date = 2010-05-04T13:20:23+02:00
draft = false
tags = [ 'Tomcat' ]
categories = [ 'Run' ]
image = 'tomcatmemoryleak.png'
+++


If you upgraded to Tomcat 6.0.26, you have seen this kind of message in your logs:

```
May 4, 2010 5:08:46 p.m. org.apache.catalina.loader.WebappClassLoader clearReferencesThreads SEVERE: A web application Appears To Have started a thread named [Thread-8] but Has Failed to stop it. This is very Likely to create a memory leak.
```

Or:

```
May 4, 2010 3:47:09 p.m. org.apache.catalina.loader.WebappClassLoader clearReferencesJdbc SEVERE: A web application registered the JDBC driver [com.ibm.as400.access.AS400JDBCDriver] but failed to unregister it When the web application was stopped. To Prevent a memory leak, the JDBC Driver has-been forcibly unregistered.
```

These messages come from a new component, the [JreMemoryLeakPreventionListener] (http://wiki.apache.org/tomcat/MemoryLeakProtection) that monitors threads and JDBC drivers created by your web applications when unloading a webapp.

It is enabled by default and can be found in the `server.xml` in the `this` conf of your Tomcat installation.

```
<! - Prevent memory leaks due to use of Particular java / javax APIs - >    
<Listener className = " " org.apache.catalina.core.JreMemoryLeakPreventionListener />
```

Even though it is not recommended to disable this control, which allows you to trace memory leaks, you can return to the previous situation without ‘monitoring’ commenting on the `Listener`.