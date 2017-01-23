Mount an ISO Image
------------------
A use case for mounting an ISO image on your router.

There's an excellent tool for downloading and creating ISO images of security updates for Microsoft Windows and Office, for offline use. And it's possible to remove any updates that you don't want installed. The ISO images may be copied to the router's external USB storage device and mounted read-only, so that all Windows computers on the network may update themselves without ever connecting directly to Microsoft's servers.

On my router, I mount the ISO images using Asuswrt-Merlin and advertise them on a Samba share. Then I run the client update tool on each computer manually, to apply the patches. Afterwards, I use the Microsoft Baseline Security Analyzer to verify that all critical updates have been applied. Here it is:

**WSUS Offline Update**  
http://www.wsusoffline.net/

Mounting an ISO image on my router is easy.
```
/bin/mount -t iso9660 -o loop /path/to/image.iso /path/to/mountpoint
```

However, you need an extra kernel module (CONFIG_ISO9660_FS=m); it must be loaded before mounting the ISO image.
```
/sbin/modprobe isofs
```

Advertise the folder containing all mounted ISO images on a Samba share.

**/jffs/scripts/smb.postconf**
```
#!/bin/sh
/usr/bin/logger -t $(/usr/bin/basename $0) "custom script started [$$]"
finish()  {
  /usr/bin/logger -t $(/usr/bin/basename $0) "custom script ended [$$]"
}
trap finish EXIT
CONFIG=$1
source /usr/sbin/helper.sh

###########################################################################
# Samba shares
/bin/cat <<EOF >> "$CONFIG"

[WSUSOFFLINE]
        comment = WSUS Offline Updates
        path = /mnt/wsusoffline
        writeable = no
        create mask = 0777
        dos filetimes = yes
        fake directory create times = yes

EOF
###########################################################################
```


Lastly, WSUS Offline should be run after each Patch Tuesday (every second Tuesday in a month). This will produce a new set of ISO images for you to publish on your local network. The file size of these ISO images is quite large. Example:
```
-r--r--r--    1 admin    root        4.7G Jan 20 22:28 wsusoffline-ofc-enu.iso
-r--r--r--    1 admin    root        4.3G Jan 20 22:26 wsusoffline-w100-x64.iso
-r--r--r--    1 admin    root        2.7G Jan 20 22:26 wsusoffline-w61-x64.iso
-r--r--r--    1 admin    root        1.7G Jan 20 22:24 wsusoffline-w61.iso
```
