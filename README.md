blackfuel / asuswrt-merlin-jffs
===============================

These Linux scripts and configuration files are to be copied to the router /jffs folder, for use with the blackfuel version of the asuswrt-merlin firmware.  It shows you how to enable real-time disk encryption and enable the GPS-backed NTP server.

Disk Encryption

Your external USB storage device may initially be partitioned and formatted on Ubuntu or any other Linux distribution with LUKS enabled.  Mine was formatted (ext4 + LUKS) using the Ubuntu disk manager.  Once partitioned and formatted, connect it to the router to see the LUKS UUIDs.

$ /entware/sbin/cryptsetup luksUUID /dev/sda1

11111111-1111-1111-1111-111111111111


$ /entware/sbin/cryptsetup luksUUID /dev/sda2

22222222-2222-2222-2222-222222222222


$ /entware/sbin/cryptsetup luksUUID /dev/sdb1

33333333-3333-3333-3333-333333333333


Update the pre-mount and services-stop scripts with these LUKS UUIDs to automatically mount the encrypted disk at boot time, and to un-mount it at shutdown. Unfortunately, there is no Asus /etc/crypttab, as seen in a modern Linux OS.  My example scripts show how to mount a disk with both an encrypted swap and encrypted data partition, and a USB flash drive with one encrypted data partition.



GPS-backed NTP server

A GPS-backed NTP server will improve the reliability of your router and give you a dead-on clock.  You will need to assemble a custom cable and enclosure for the GPS module.  All the parts can probably be purchased for less than $50.  I got the idea from the following forum post.

http://www.snbforums.com/threads/set-the-router-clock-with-a-gps-receiver-for-under-70.26981/page-4#post-213194

The ntp.conf configuration file shows how to configure the generic NEMA driver for NTP and the ntp-start script shows how to integrate it with the firmware.

