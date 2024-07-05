+++
title = 'Apple JDK and OpenJDK - Back to Benchs'
date = 2011-04-16T13:20:23+02:00
draft = false
tags = [ 'Benchmarks', 'OpenJDK', 'OSX'  ]
categories = [ 'OS', 'Performance' ]
image = 'osxbenchopenjdk.png'
+++

It’s good to see works in progress for Aqua/Cocoa - AWT ports but what about JVM performances ?

## Test vms

I selected 4 VMs to be tested

`Apple Java 1.6.0_22 - java version "1.6.0_22" Java(TM) SE Runtime Environment (build 1.6.0_22-b04-314-10M3406a) Java HotSpot(TM) 64-Bit Server VM (build 17.1-b03-314, mixed mode)`

`Apple Java 1.6.0_24 - java version "1.6.0_24" Java(TM) SE Runtime Environment (build 1.6.0_24-b07-348-10M3406a) Java HotSpot(TM) 64-Bit Server VM (build 19.1-b02-348, mixed mode)`

`OpenJDK 7 bsd-port - openjdk version "1.7.0-internal" OpenJDK Runtime Environment (build 1.7.0-internal-henri_2011_04_11_08_24-b00) OpenJDK 64-Bit Server VM (build 21.0-b07, mixed mode)`

`OpenJDK 7 macosx-port - openjdk version "1.7.0-internal" OpenJDK Runtime Environment (build 1.7.0-internal-b00) OpenJDK 64-Bit Server VM (build 21.0-b07, mixed mode)`

## Test system

My test system is an Apple iMac (iMac11,1 ) with Intel i7 2.80Ghz and 8Gb DDR3 1067Mhz, running under SnowLeopard 10.6.7 64bits. I wanted to test 64bits VMs on a 64bits machine and this time use a stronger processor with more threads (ie: 4 cores with hyperthreading).

### DaCapo Benchmarks

I keep the [DaCapo 9.12-bach](http://www.dacapobench.org/).

Bench tests launched with -n X, ie (java -jar dacapo-9.12-bach.jar -n 10 pmd)

| Bench                     | Apple JDK6 b22 | Apple JDK6 b24 | OpenJDK 1.7 bsd-port     | OpenJDK 1.7 macosx-port |
| ------------------------- | -------------- | -------------- | ------------------------ | ----------------------- |
| avrora (10 iterations)    | 3464ms         | 3406ms         | 3281ms                   | 3410ms                  |
| eclipse (2 iterations)    | 25635ms        | 23264ms        | 22156ms                  | 23503ms                 |
| fop (10 iterations)       | 379ms          | 351ms          | 301ms                    | 305ms                   |
| h2 (2 iterations)         | 5662ms         | 5308ms         | 4557ms                   | 4694ms                  |
| jython (2 iterations)     | 4287ms         | 4188ms         | Failure (Trace/BPT trap) | 4004ms                  |
| luindex (10 iterations)   | 2402ms         | 763ms          | 623ms                    | 670ms                   |
| lusearch (10 iterations)  | 1500ms         | 2173ms         | 1190ms                   | 4019ms                  |
| pmd (10 iterations)       | 2054ms         | 1860ms         | 1582ms                   | 1891ms                  |
| sunflow (10 iterations)   | 2763ms         | 2658ms         | 2342ms                   | 2292ms                  |
| tomcat (5 iterations)     | 1943ms         | 1884ms         | 1653ms                   | 1778ms                  |
| tradebeans (5 iterations) | 6702ms         | 6199ms         | 4968ms                   | 5080ms                  |
| tradesoap (5 iterations)  | 20058ms        | 18501ms        | 21850ms                  | 20114ms                 |
| xalan (10 iterations)     | 1080ms         | 926ms          | 788ms                    | 805ms                   |

## Conclusion

Latest Apple JVM, 1.6.0-24 perform better than the old 1.6.0-22 in all of the tests and is near OpenJDK 7 results.

OpenJDK 7 from the bsd-port perform a little better than the macosx-port. The main difference in build is bsd-port is using stock gcc whereas macos-port use llvm-gcc.

### bsd-port using stock-gcc during OpenJDK build

```
Compiling /Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-bsdport-x86_64/workspace/hotspot/src/share/vm/adlc/arena.cpp rm -f ../generated/adfiles/arena.o /Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-bsdport-x86_64/workspace/ALT_COMPILER_PATH/g++ -D_ALLBSD_SOURCE -D_GNU_SOURCE -DAMD64 -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-bsdport-x86_64/workspace/hotspot/src/share/vm/prims -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-bsdport-x86_64/workspace/hotspot/src/share/vm -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-bsdport-x86_64/workspace/hotspot/src/cpu/x86/vm -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-bsdport-x86_64/workspace/hotspot/src/os_cpu/bsd_x86/vm -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-bsdport-x86_64/workspace/hotspot/src/os/bsd/vm -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-bsdport-x86_64/workspace/hotspot/src/os/posix/vm -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-bsdport-x86_64/workspace/hotspot/src/share/vm/adlc -I../generated -DASSERT -DTARGET_OS_FAMILY_bsd -DTARGET_ARCH_x86 -DTARGET_ARCH_MODEL_x86_64 -DTARGET_OS_ARCH_bsd_x86 -DTARGET_OS_ARCH_MODEL_bsd_x86_64 -DTARGET_COMPILER_gcc -DCOMPILER2 -DCOMPILER1 -fno-rtti -fno-exceptions -pthread -fcheck-new -m64 -pipe -Werror -g -c -o ../generated/adfiles/arena.o /Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-bsdport-x86_64/workspace/hotspot/src/share/vm/adlc/arena.cpp
```

### macosx-port using llvm-gcc during OpenJDK build

```
Compiling /Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-macosx-universal/workspace/hotspot/src/share/vm/adlc/arena.cpp rm -f ../generated/adfiles/arena.o llvm-g++ -D_ALLBSD_SOURCE -D_GNU_SOURCE -DIA32 -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-macosx-universal/workspace/hotspot/src/share/vm/prims -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-macosx-universal/workspace/hotspot/src/share/vm -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-macosx-universal/workspace/hotspot/src/cpu/x86/vm -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-macosx-universal/workspace/hotspot/src/os_cpu/bsd_x86/vm -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-macosx-universal/workspace/hotspot/src/os/bsd/vm -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-macosx-universal/workspace/hotspot/src/os/posix/vm -I/Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-macosx-universal/workspace/hotspot/src/share/vm/adlc -I../generated -DASSERT -DTARGET_OS_FAMILY_bsd -DTARGET_ARCH_x86 -DTARGET_ARCH_MODEL_x86_32 -DTARGET_OS_ARCH_bsd_x86 -DTARGET_OS_ARCH_MODEL_bsd_x86_32 -DTARGET_COMPILER_gcc -DCOMPILER2 -DCOMPILER1 -fno-rtti -fno-exceptions -pthread -fcheck-new -m32 -march=i586 -mstackrealign -pipe -Werror -g -c -o ../generated/adfiles/arena.o /Users/henri/Documents/jenkins/data/jobs/openjdk-1.7-macosx-universal/workspace/hotspot/src/share/vm/adlc/arena.cpp
```

Performances gain in OpenJDK7 VM vs latest Apple 6 VM is smaller than previously (see previous articles on Apple JDK vs OpenJDK 6), switching to OpenJDK 7 will not be only for pure speed but for functionalities.