How to install/set-up OpenWrt on a D-Link DIR-505L wireless router
This guide is more for myself, but feel free to ask questions.

OpenWrt wiki page for the DIR-505: http://wiki.openwrt.org/toh/d-link/dir-505

A. Basic setup and housekeeping:

Follow steps B thru E in this guide: https://dl.dropboxusercontent.com/u/57289645/blog/dir-505l_openwrt_setup_guide.txt

    # ethernet
    uci set network.wan=interface
    uci set network.wan.proto=dhcp
    uci set network.wan.ifname=eth1
    uci del network.lan.ifname
    # wifi
    uci set wireless.@wifi-device[0].disabled=0
    uci set wireless.@wifi-iface[0].ssid="your_ssid"
    uci set wireless.@wifi-iface[0].encryption="psk2"
    uci set wireless.@wifi-iface[0].key="your_password"
    # system
    uci set system.@system[0].hostname="your_hostname"
    uci set system.@system[0].timezone="PST8PDT,M3.2.0,M11.1.0" # Los Angeles
    # commit changes
    uci commit
    wifi

Sources:
https://forum.openwrt.org/viewtopic.php?pid=230861#p230861
http://wiki.openwrt.org/doc/uci/wireless/encryption#configure.wpa2.psk
http://wiki.openwrt.org/doc/uci/system

B. USB storage support (assuming NTFS-formatted USB stick):

    opkg update
    opkg install kmod-usb-storage kmod-fs-ntfs ntfs-3g
    mkdir -p /mnt/usb_drive
    ntfs-3g /dev/sda1 /mnt/usb_drive -o rw,sync
    # do stuff
    umount /dev/sda1

Sources:
http://wiki.openwrt.org/doc/howto/usb.essentials
http://wiki.openwrt.org/doc/howto/usb.storage
http://wiki.openwrt.org/doc/howto/writable_ntfs

C. USB serial adapter support:

    opkg update
    opkg install kmod-usb-serial coreutils-stty
    opkg install kmod-usb-serial-pl2303 # for Prolific PL2303-based devices
    opkg install kmod-usb-acm # for devices using Abstract Control Model (ACM)
    # reboot here
    cat /dev/ttyUSB0 # view data coming to the serial port
    stty -F /dev/ttyUSB0 -a # view serial port settings

Source:
http://wiki.openwrt.org/doc/hardware/port.serial#usb.enabled.routers

D. Minimal Python install with pyserial support

    opkg update
    opkg install python-mini pyserial
    # termios.so is missing so we need to copy it from the full package
    cd /tmp
    opkg download python
    tar -xzf python_*.ipk ./data.tar.gz
    tar -xzf data.tar.gz ./usr/lib/python2.7/lib-dynload/termios.so -C /

Source:
http://kelvinsthunderstorm.com/omnimaopenwrt-and-xbee/
