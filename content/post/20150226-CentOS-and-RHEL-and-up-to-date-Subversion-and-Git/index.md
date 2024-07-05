+++
title = 'CentOS & RHEL and up to date Subversion and Git'
date = 2015-02-26T13:20:23+02:00
draft = false
tags = [ 'CentOS', 'Git', 'Subversion' ]
categories = [ 'OS' ]
image = 'git-svn-centos-rhel.png'
+++

If you’re using CentOS/RHEL 5, 6 or 7, you have noticed bundled subversion and git are quite old. It could be a problem if you need to use smart features, like Sparse Checkout. Also Jenkins Git Plugin recommand version 1.7 or higher.

Of course, you could recompile git from sources, but it’s better to use prebuilt packages.

### Here enter Wandisco ![wandisco](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/WANdisco_logo.svg/251px-WANdisco_logo.svg.png?20161116231409)

Wandisco provides up to date Subversion RPMs for CentOS 5/6/7 :

* [Subversion 1.7 for CentOS 5](http://opensource.wandisco.com/centos/5/svn-1.7/)
* [Subversion 1.8 for CentOS 5](http://opensource.wandisco.com/centos/5/svn-1.8/) 
* [Subversion 1.9 for CentOS 5](http://opensource.wandisco.com/centos/5/svn-1.9/)

* [Subversion 1.7 for CentOS 6](http://opensource.wandisco.com/centos/6/svn-1.7/)
* [Subversion 1.8 for CentOS 5](http://opensource.wandisco.com/centos/6/svn-1.8/)
* [Subversion 1.9 for CentOS 6](http://opensource.wandisco.com/centos/6/svn-1.9/)

* [Subversion 1.7 for CentOS 7](http://opensource.wandisco.com/centos/7/svn-1.7/)
* [Subversion 1.8 for CentOS 7](http://opensource.wandisco.com/centos/7/svn-1.8/)
* [Subversion 1.9 for CentOS 7](http://opensource.wandisco.com/centos/7/svn-1.9/)

Better, they also offer quite fresh Git RPMs too :

* [Git for CentOS 5](http://opensource.wandisco.com/centos/5/git/) - 2.2.0
* [Git for CentOS 6](http://opensource.wandisco.com/centos/6/git/) - 2.2.1 
* [Git for CentOS 7](http://opensource.wandisco.com/centos/7/git/) - 2.1.0

You could download RPM and install it but better is to add Wandisco repository to yum repositories :

Example to use Wandisco Git on CentOS/RHEL 6 :

```
#Wandisco-Git.repo
[wandisco-Git]
name=CentOS-6 - Wandisco Git
baseurl=http://opensource.wandisco.com/centos/6/git/$basearch/
enabled=1
gpgcheck=1
gpgkey=http://opensource.wandisco.com/RPM-GPG-KEY-WANdisco
```

Example to use Wandisco Subversion 1.9 on CentOS/RHEL 5 :

```
#Wandisco-SVN-1.9.repo
[wandisco-Subversion-1.9]
name=CentOS-5 - Wandisco Subversion 1.9
baseurl=http://opensource.wandisco.com/centos/5/svn-1.9/RPMS/$basearch/
enabled=1
gpgcheck=1
gpgkey=http://opensource.wandisco.com/RPM-GPG-KEY-WANdisco
```


Save the earth, reduce your CO2 impact, and stick on prebuilt-native packages :)