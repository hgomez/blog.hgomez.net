+++
title = 'Apple JDK and OpenJDK – Core2Duo vs I7'
date = 2011-04-13T13:20:23+02:00
draft = false
tags = [ 'Benchmarks', 'OpenJDK', 'OSX'  ]
categories = [ 'OS' ]
image = 'c2cvsi7openjdk.png'
+++

Previously I did benchmark of Apple VMs and OpenJDK 6 and I wanted to see how all of the JVMs available today on our Mac on two systems, an old Core2Duo and a newer i7. And also see how they perform 32 / 64 bits kernel mode.

So I redo full dacapo bench suite to include OpenJDK 6, and we have now 5 VMs (3 Java 6 and 2 Java 7) :

- Apple Java 1.6.0_22 - Java HotSpot(TM) 64-Bit Server VM (build 17.1-b03-314, mixed mode)
    
- Apple Java 1.6.0_24 - Java HotSpot(TM) 64-Bit Server VM (build 19.1-b02-348, mixed mode)
    
- OpenJDK 7 bsd-port - OpenJDK 64-Bit Server VM (build 21.0-b07, mixed mode)
    
- OpenJDK 7 macosx-port - OpenJDK 64-Bit Server VM (build 21.0-b07, mixed mode)
    
- OpenJDK 6 macports - OpenJDK 64-Bit Server VM (build 17.0-b16, mixed mode)
    

### Results on MacBook Pro - Core2Duo 2.66Ghz - 32bits kernel

|Bench|Apple JDK6 b22|Apple JDK6 b24|OpenJDK 1.7 bsd-port|OpenJDK 1.7 macosx-port|OpenJDK 1.6|
|---|---|---|---|---|---|
|avrora (10 iterations)|5436|5246|4917|5059|5061|
|eclipse (2 iterations)|49442|49529|37131|43572|37292|
|fop (10 iterations)|561|519|395|398|456|
|h2 (2 iterations)|7204|6635|6312|6341|11051|
|jython (2 iterations)|6517|5928||5947||
|luindex (10 iterations)|1095|2170|1014|985|953|
|lusearch (10 iterations)|7764|4379|5077|7611|5534|
|pmd (10 iterations)|3178|3295|2475|3438|2437|
|sunflow (10 iterations)|6969|7038|6543|6566|6564|
|tomcat (5 iterations)|4024|3924|3571|3820||
|tradebeans (5 iterations)|8028|7516|5851|5914|5954|
|tradesoap (5 iterations)|16839|14603|12477|13096|12943|
|xalan (10 iterations)|3128|2744|2917|3783|2816|

![Bench Core2Duo](benchc2d.png)

### Results on iMac - Core i7 2.8Ghz - 64bits kernel

| Bench                     | Apple JDK6 b22 | Apple JDK6 b24 | OpenJDK 1.7 bsd-port | OpenJDK 1.7 macosx-port | OpenJDK 1.6 |
| ------------------------- | -------------- | -------------- | -------------------- | ----------------------- | ----------- |
| avrora (10 iterations)    | 3452           | 3349           | 3505                 | 3600                    | 3269        |
| eclipse (2 iterations)    | 26081          | 24506          | 20664                | 22676                   | 23706       |
| fop (10 iterations)       | 392            | 354            | 299                  | 303                     | 324         |
| h2 (2 iterations)         | 5559           | 5341           | 4814                 | 4752                    | 8766        |
| jython (2 iterations)     | 4204           | 4168           |                      | 4094                    |             |
| luindex (10 iterations)   | 2076           | 763            | 620                  | 2226                    | 643         |
| lusearch (10 iterations)  | 1484           | 2101           | 2998                 | 3055                    | 1168        |
| pmd (10 iterations)       | 2055           | 1884           | 1614                 | 1876                    | 1635        |
| sunflow (10 iterations)   | 2808           | 2689           | 2315                 | 2267                    | 2276        |
| tomcat (5 iterations)     | 1935           | 1850           | 1772                 | 1751                    |             |
| tradebeans (5 iterations) | 6633           | 6127           | 5135                 | 5092                    | 5242        |
| tradesoap (5 iterations)  | 19250          | 18265          | 19443                | 20217                   | 22195       |
| xalan (10 iterations)     | 1067           | 1673           | 762                  | 791                     | 775         |
![Bench i7](benchi7.png)
## Conclusion

As seen if [previous article](http://blog.hgomez.net/2011/04/16/apple-jdks-openjdk-back-to-benchs/), latest Apple JVM, 1.6.0-24 perform better than the old 1.6.0-22, and still behind OpenJDK 7 and even OpenJDK 6. OpenJDK 7 bsd-port is still faster (by a small factor) than OpenJDK 7 from macosx-port (built with LLVM), in both simple threaded (Core2Duo, 2 cores) and large threaded (i7 4 cores with hyperthreading).

This benchmark show how good is [Intel Core i7](http://ark.intel.com/Product.aspx?id=41316) comparing to previous generation [Intel Core2Duo](http://ark.intel.com/Product.aspx?id=37130&code=T9550), roughly twice as fast.