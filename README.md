hostapd-mana @ Pineapple NANO + TETRA / OpenWRT
===================================
hostapd-mana patches by Dominic White (singe) & Ian de Villiers @ sensepost (research@sensepost.com)  
ported to OpenWRT by: Andreas Nilsen @adde88

Last Update: 13.01.2017
-----------------------
Installation script in place. Type this to install mana-toolkit for the Pineapple:  
```bash
wget -qO- https://raw.githubusercontent.com/adde88/hostapd-mana/master/INSTALL.sh | bash -s -- -v -v
```
  
Also:  
New improved launch script!  
Simply type 'launch-mana' (without the quotes) to start the attack.  
More to come... :)   

Yours truly, Andreas Nilsen. - @adde88


Overview
--------
A access point (evilAP) first presented at Defcon 22.  

More specifically, it contains the improvements to the KARMA attacks we implemented into hostapd, as well as the ability to rogue EAP access points.  

Thanks to TarlogicSecurity who made the hostapd-wpe patches for OpenWRT, and inspired me to get the MANA patches running on OpenWRT.  

This will attempt to track the hostapd-mana releases from Sensepost.

Contents
--------
It contains:
* hostapd-mana - modified hostapd that implements our new karma attacks
* crackapd - a tool for offloading the cracking of EAP creds to an external tool and re-adding them to the hostapd EAP config (auto crack 'n add)
* sslstrip2 & dns2proxy
* net-creds
* firelamb

Dependencies
------------
Dependencies: libubus, tinyproxy, stunnel, ip   
(You should run: "opkg update" before installing.)


Installation
------------
Install both IPK files located within the folder ./bin/ar71xx/packages/base/  
hostapd-mana_2016-09-16_ar71xx.ipk  
asleap_2.2-1_ar71xx.ipk

Everything will be installed to your usual folders:  
/usr/share/mana-toolkit/  
/etc/mana-toolkit/  
/var/lib/mana-toolkit/


Running
-------
The current startup-script: /usr/share/mana-toolkit/run-mana/mana-pineapple.sh  
It is currently stripped a bit because of dev. reasons.  
It gets hostapd-mana up and running with a DHCP server, while forwarding traffic between br-lan - wlan1. (NAT-mode)  
SSLstrip, SSLsplit, dns2proxy, crackapd.py, asleap is NOT started. (But they should run in theory.)


License
-------
The patches included in hostapd-mana by SensePost are licensed under a Creative Commons Attribution-ShareAlike 4.0 International License (http://creativecommons.org/licenses/by-sa/4.0/) Permissions beyond the scope of this license may be available at http://sensepost.com/contact us/. hostapd's code retains it's original license available in COPYING.




