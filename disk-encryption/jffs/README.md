Disk Encryption
---------------
How to enable real-time disk encryption on your router with the Blackfuel version of Asuswrt-Merlin firmware.

Unfortunately, there is no Asus /etc/crypttab, as with a modern Linux OS.  My example scripts show how to mount a disk with both an encrypted swap and an encrypted data partition, and a USB flash drive with one encrypted data partition.

Since the mounting and unmounting of encrypted partitions is not automatically done by the firmware, we must do it ourselves.  Additionally, we must also handle Entware start/stop and Samba start/stop.  The scripts show how Entware is started and stopped when it is installed on the encrypted partition.  Samba must also be started and stopped manually.

1. Copy the scripts and configuration files, found in this folder, to the appropriate /jffs folders on your router.  You should know where they go.

2. Find out the LUKS UUIDs of your encrypted USB storage devices.  Your external USB storage device may initially be partitioned and formatted on Ubuntu or any other Linux distribution with LUKS enabled.  Mine was formatted (ext4 + LUKS) using the Ubuntu disk manager.  Once partitioned and formatted, connect it to the router to see the LUKS UUIDs.

    **Example**
    
    **`$ /entware/sbin/cryptsetup luksUUID /dev/sda1`**
    
    *`11111111-1111-1111-1111-111111111111`*

    **`$ /entware/sbin/cryptsetup luksUUID /dev/sda2`**

    *`22222222-2222-2222-2222-222222222222`*

    **`$ /entware/sbin/cryptsetup luksUUID /dev/sdb1`**

    *`33333333-3333-3333-3333-333333333333`*

3. Update the pre-mount and services-stop scripts with these LUKS UUIDs to automatically mount the encrypted disk at boot time, and to un-mount it at shutdown.
