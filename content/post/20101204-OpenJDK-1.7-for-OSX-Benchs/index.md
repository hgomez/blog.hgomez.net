+++
title = 'OpenJDK 1.7 for OSX Benchs'
date = 2010-12-04T13:20:23+02:00
draft = false
tags = [ 'Benchmarks', 'OpenJDK', 'OSX'  ]
categories = [ 'OS' ]
image = 'dacapoopenjdk.png'
+++

After building and packaging OpenJDK 1.7 for OS/X, I wanted to see how performed new VMs.

## Test vms

Recents OpenJDK 1.7 32 and 64bits where used :

`openjdk version "1.7.0-internal" OpenJDK Runtime Environment (build 1.7.0-internal-henri_2010_12_01_00_46-b00) OpenJDK Server VM (build 20.0-b02, mixed mode)`

`openjdk version "1.7.0-internal" OpenJDK Runtime Environment (build 1.7.0-internal-henri_2010_12_01_00_49-b00) OpenJDK 64-Bit Server VM (build 20.0-b02, mixed mode)`

## Test system

My test system is an Apple Mac Book Pro (MacBookPro5,1) with Intel Core 2 Duo 2.66Ghz and 4Gb DDR3 1067Mhz, running under SnowLeopard 10.6.5 32bits.

### DaCapo Benchmarks

I used again [DaCapo 9.12-bach](http://www.dacapobench.org/), discarding **batik** test, this one requiring a working AWT/Swing support .

Bench tests launched with -n X, ie (java -jar dacapo-9.12-bach.jar -n 10 pmd)

| Bench                     | Apple JDK6             | OpenJDK 6                  | OpenJDK 1.7 32bits       | OpenJDK 1.7 64bits       |
| ------------------------- | ---------------------- | -------------------------- | ------------------------ | ------------------------ |
| avrora (10 iterations)    | 5247ms                 | 4980ms                     | 6592ms                   | 4808ms                   |
| eclipse (2 iterations)    | 53292ms                | 34404ms                    | 40996ms                  | 38335ms                  |
| fop (10 iterations)       | 560ms                  | 408ms                      | 505ms                    | 383ms                    |
| h2 (2 iterations)         | Failure (pending test) | 6488ms                     | 6876ms                   | 5557ms                   |
| jython (2 iterations)     | 6034ms                 | Failure (Trace/BPT trap)   | Failure (Trace/BPT trap) | Failure (Trace/BPT trap) |
| luindex (10 iterations)   | 1072ms                 | 990ms                      | 1034ms                   | 967ms                    |
| lusearch (10 iterations)  | 5997ms                 | 3957ms                     | 4884ms                   | 3895ms                   |
| pmd (10 iterations)       | 3067ms                 | 2890ms                     | 3085ms                   | 2188ms                   |
| sunflow (10 iterations)   | 6998ms                 | 6442ms                     | 6898ms                   | 6719ms                   |
| tomcat (5 iterations)     | 4108ms                 | Failure (connection reset) | 4041ms                   | 3698ms                   |
| tradebeans (5 iterations) | 8257ms                 | Failure (connection reset) | 6384ms                   | 5545ms                   |
| tradesoap (5 iterations)  | 20472ms                | 12378ms                    | 16250ms                  | 13583ms                  |
| xalan (10 iterations)     | 2877ms                 | 2847ms                     | 3071ms                   | 2875ms                   |
## Conclusion

Good news, two tests **Tomcat** and **tradebeans** now pass the bench

OpenJDK 1.7 64bits perform better than OpenJDK 1.7 32bits and OpenJDK 6.

Even if OpenJDK 1.7 32bits performances are better than Apple Java 6, it’s allways behind OpenJDK 1.7 64bits version on OS/X, so you should select the 64bits version if performance is the key for your use.

Next article will cover OpenJDK 1.7 and [jtreg](http://openjdk.java.net/jtreg/), the Regression Test Harness for the OpenJDK platform.