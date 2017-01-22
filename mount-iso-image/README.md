Mount an ISO Image
------------------
Use case for mounting an ISO image and advertising it on a Samba share using the Blackfuel version of Asuswrt-Merlin firmware.

There's an excellent tool for downloading and creating ISO images for all Microsoft Windows and Office updates. And it's possible to remove any updates you don't want installed. The ISO images may be copied to the router's external USB storage device and mounted read-only, so that all the Windows computers on the network may update themselves without connecting directly to Microsoft servers.

On my router, I mount the ISO images using Asuswrt-Merlin and advertise them on a Samba share. Then I run the update tool on each computer manually. Afterwards I run the Microsoft Baseline Security Analyzer to verify that all critical updates have been applied. Here it is:

**WSUS Offline Update**
http://www.wsusoffline.net/

**Mounting an ISO image in Asuswrt-Merlin is easy**
'/bin/mount -t iso9660 -o loop /path/to/image.iso /path/to/mountpoint'

**Just need an extra module (CONFIG_ISO9660_FS=m) because it must be loaded before mounting the ISO image.**
'/sbin/modprobe isofs'
