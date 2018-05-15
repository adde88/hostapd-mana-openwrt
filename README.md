hostapd-mana 2.6 @ Pineapple NANO + TETRA / OpenWRT
===================================
hostapd-mana patches by Dominic White (singe) & Ian de Villiers @ sensepost (research@sensepost.com)  
ported to OpenWRT by: Andreas Nilsen @adde88

Last Update: 06.01.2018
-----------------------
Updated to work properly with the latest FW updates to the WiFi Pineapples!
Type this to install mana-toolkit for the Pineapple:  
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
Dependencies: libubus, tinyproxy, stunnel, ip, python, openssl   
(You should run: "opkg update" before installing.)


Installation
------------
Install both IPK files located within the folder ./bin/ar71xx/packages/base/  
hostapd-mana_2.6-5_ar71xx.ipk  
asleap_2.2-1_ar71xx.ipk

Everything will be installed to your usual folders:  
/usr/share/mana-toolkit/  
/etc/mana-toolkit/  
/var/lib/mana-toolkit/

License
-------
The patches included in hostapd-mana by SensePost are licensed under a Creative Commons Attribution-ShareAlike 4.0 International License (http://creativecommons.org/licenses/by-sa/4.0/) Permissions beyond the scope of this license may be available at http://sensepost.com/contact us/. hostapd's code retains it's original license available in COPYING.




