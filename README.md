The repo. contains the OpenWRT SDK files for building hostapd with the MANA patches made by Sensepost
It also contains the binary installation files, to install it on your Pineapple NANO + TETRA.

Dependencies: libubus, tinyproxy, stunnel, ip
(You should run: "opkg update" before installing.)

Just install both IPK files located within the folder ./bin/ar71xx/packages/base/
hostapd-mana_2016-09-16_ar71xx.ipk
asleap_2.2-1_ar71xx.ipk

Everything will be installed to the usual folders:
/usr/share/mana-toolkit/
/etc/mana-toolkit/
/var/lib/mana-toolkit/

The current startup-script: /usr/share/mana-toolkit/run-mana/mana-pineapple.sh
It is currently stripped a bit for dev. reasons. It gets hostapd-mana up and running with a DHCP server, while forwarding traffic between br-lan - wlan1.
SSLstrip, SSLsplit, dns2proxy, crackapd.py, asleap is NOT started. (But should run in theory.)

Stay tuned for updates, as the goal is to get all parts of MANA toolkit running from your pocket.
Yours truly, Andreas Nilsen. - @adde88
