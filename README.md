hostapd-mana 2.6.5
===================================
hostapd with MANA patches by Dominic White (singe) & Ian de Villiers @ sensepost (research@sensepost.com)  
Ported to OpenWRT by: Andreas Nilsen @adde88

Last Update: 30.01.2019
-----------------------
Updated to follow the upstream changes from Sensepost.  
Added a 'light' version, that ONLY contains hostapd-mana binaries.  
Also added IPK's and wpa_sycuphant binaries for both Chaos Calmer and snapshot OpenWRT. (TARGET=PINEAPPLE-NANO)  
Type this to install mana-toolkit on the WiFi Pineapple NANO/TETRA:  
```bash
wget -qO- https://raw.githubusercontent.com/adde88/hostapd-mana/master/INSTALL.sh | bash -s -- -v -v
```
  
Also:  
Improved launch wrapper!  
Simply type 'launch-mana' (without the quotes) to start the MANA attack.  
More coming...  
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
(You should run: "opkg update" before installing any IPK's.)


Manual Installation
------------
Run 'opkg update' first.
Transfer and install the following IPK files located within the sub-folder ./bin/ar71xx/packages/base/ to your device:  
hostapd-mana_2.6.5-4_ar71xx.ipk  
asleap_2.2-1_ar71xx.ipk

Will be installing to the usual folders:  
/usr/share/mana-toolkit/  
/etc/mana-toolkit/  
/var/lib/mana-toolkit/

License
-------
The patches included in hostapd-mana by SensePost are licensed under a Creative Commons Attribution-ShareAlike 4.0 International License (http://creativecommons.org/licenses/by-sa/4.0/) Permissions beyond the scope of this license may be available at http://sensepost.com/contact us/. hostapd's code retains it's original license available in COPYING.




