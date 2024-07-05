+++
title = 'How to Fix HP Laserjet 1102w printerAgent Using Port 8080 on Mac ?'
date = 2010-09-25T13:20:23+02:00
draft = false
tags = [ 'JLaserJet', 'OSX' ]
categories = [ 'OS' ]
image = '11028080back.png'
+++

I recently bought a new Laser printer, a HP Laserjet 1102w. A very good printer bundled with a very rich firmware supporting WIFI Wep/WPA/WPA2, Bonjour, SNMP v1/v2, a web interface and much more. A definitive good choice but with a real problem for the Java developer.

Why ? Because HP printer agent is using the 8080 port, the default http port for Tomcat, JBoss and others servlet engines/application servers.

Of course, you could update Tomcat or JBoss default listen ports but you could also try to update the HP printerAgent default port. There is no way for now from the UI but you could hack the driver properties.

The settings live under user directory, ie : ~/Library/LaunchAgents/com.hp.printerAgent.plist


```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN"
"http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
       <key>Label</key>
       <string>com.hp.printerAgent</string>
       <key>OnDemand</key>
       <false/>
       <key>Program</key>
       <string>/Library/Printers/hp/laserjet/P1100_1560_1600Series/printerAgent</string>
       <key>ProgramArguments</key>
       <array>
       <string>/Library/Printers/hp/laserjet/P1100_1560_1600Series/printerAgent</string>
       </array>
       <key>RunAtLoad</key>
       <true/>
       <key>ServiceIPC</key>
       <true/>
       <key>Sockets</key>
       <dict>
               <key>MyListenerSocket</key>
               <dict>
                       <key>SockServiceName</key>
                       <string>8080</string>
               </dict>
       </dict>
</dict>
</plist>
```



Just change the **SockServiceName** from 8080 to another port, ie 58080.

Use a Text editor or a good old sed :

```
sudo sed -i “” -e “s|8080|58080|g” ~/Library/LaunchAgents/com.hp.printerAgent.plist
```

Restart your Mac and check the ports in listening mode :

```
MacBook-Pro-de-Rico:~ henri$ netstat -an | grep LISTEN
tcp6       0      0  *.3689                 *.*                    LISTEN
tcp4       0      0  *.3689                 *.*                    LISTEN
tcp4       0      0  *.88                   *.*                    LISTEN
tcp6       0      0  *.88                   *.*                    LISTEN
tcp4       0      0  *.3306                 *.*                    LISTEN
tcp46      0      0  *.80                   *.*                    LISTEN
tcp46      0      0  *.5900                 *.*                    LISTEN
tcp4       0      0  *.58080                *.*                    LISTEN
tcp6       0      0  *.58080                *.*                    LISTEN
tcp46      0      0  *.5204                 *.*                    LISTEN
tcp4       0      0  *.5204                 *.*                    LISTEN
tcp4       0      0  *.22                   *.*                    LISTEN
tcp6       0      0  *.22                   *.*                    LISTEN
tcp4       0      0  *.139                  *.*                    LISTEN
tcp4       0      0  *.445                  *.*                    LISTEN
tcp4       0      0  *.548                  *.*                    LISTEN
tcp6       0      0  *.548                  *.*                    LISTEN
tcp4       0      0  127.0.0.1.631          *.*                    LISTEN
tcp6       0      0  ::1.631                *.*                    LISTEN
```
Et voila