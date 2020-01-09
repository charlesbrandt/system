be sure to label the drive externally
when it was first initialized
what file system
capacity (in case it's not clear from the drive)


see what device path gets assigned after mount:

    dmesg

to determine current file system format:

    mount

or even:

    sudo fdisk /dev/sdb

FAT32 == /dev/sdb1 on /media/charles/8698-D9F8 type vfat

Device     Boot Start       End   Sectors   Size Id Type
/dev/sdb1          32 240254975 240254944 114.6G  c W95 FAT32 (LBA)


to wipe first:

    sudo dd if=/dev/urandom of=/dev/sdb bs=1M


looking into Advanced Format drives
wdc.com/advformat

they recommend parted (2.2 and up) instead of fdisk:

    parted -v

this starts in an interactive mode similar to fdisk:

    sudo parted -a optimal /dev/sdb


can also supply commands via command line:

```
sudo ls #prime sudo

sudo parted -a optimal /dev/sdb mklabel gpt
# for bootable usb flash drives
sudo parted -a optimal /dev/sdb mklabel msdos
sudo parted -a optimal /dev/sdb print

sudo parted -a optimal /dev/sdb rm 1
sudo parted -a optimal /dev/sdb rm 2

sudo parted -a optimal /dev/sdb mkpart primary NTFS 0% 100%
sudo mkfs.ntfs -L DATA -f /dev/sdb1

#old options:
#sudo parted -a optimal /dev/sdb mkpart primary NTFS 1MB 2000GB
(13.04: http://charlesmccolm.com/2013/06/14/making-ntfs-partitions-in-linux
sudo mkntfs -L DATA -f /dev/sdb1

or
sudo parted -a optimal /dev/sdb mkpart primary hfs 1MB 2000GB
format on mac

or
sudo parted -a optimal /dev/sdb mkpart primary ext4 1MB 2000GB
sudo mkfs.ext4 -L backups /dev/sdb1

or
sudo parted -a optimal /dev/sdb mkpart primary fat32 1MB 100%
sudo mkfs.vfat -n UBUNTU /dev/sdb1

```

// parted doesn't understand exfat. use gdisk option below
// sudo parted -a optimal /dev/sdb mkpart primary exfat 0% 100%
`
https://matthew.komputerwiz.net/2015/12/13/formatting-universal-drive.html

sudo gdisk /dev/sdb

First, create a new GPT partition table

    Command (? for help): o
    This option deletes all partitions and creates a new protective MBR.
    Proceed? (Y/N): Y

Now create a partition. The defaults will create a new partition that spans the whole drive with the first sector already aligned. Be sure to choose the correct type 0700!

Command (? for help): n
Partition number (1-128, default 1):
First sector (34-16326462, default = 2048) or {+-}size{KMGTP}:
Last sector (2048-16326462, default = 16326462) or {+-}size{KMGTP}:
Current type is 'Linux filesystem'
Hex code or GUID (L to show codes, Enter = 8300): 0700
Changed type of partition to 'Microsoft basic data'
Write the changes to the drive and exit

Command (? for help): w

Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
PARTITIONS!!

Do you want to proceed? (Y/N): Y
OK; writing new GUID partition table (GPT) to /dev/sdX.
Warning: The kernel is still using the old partition table.
The new table will be used at the next reboot.
The operation has completed successfully.
Finally, format our new partition with the exFAT filesystem.

% sudo mkfs.exfat /dev/sdX1
mkexfatfs 1.0.1
Creating... done.
Flushing... done.
File system created successfully.

sudo mkfs.exfat -n Storage /dev/sdb1

(android still doesn't like this as of 2019.07.17 09:43:58, pixel3. Just using android to reformat the flash drive yields: vfat (fat32))






*2012.10.09 15:00:34 hfs_plus
on os x 10.8 (10.7+) they've eliminated formatting drive as "Mac OS Extended" (non-journaled).

Linux cannot write to HFS+ Journal, so must diable Journal:
To get around this:

You must partition it journaled; then select the partition in Disk Utility's sidebar, press the Option key before selecting File from the menubar, and you'll see a Disable Journaling option.

https://discussions.apple.com/thread/3232454?start=0&tstart=0


*2015.11.24 20:48:39
if you get the message:

Warning: The resulting partition is not properly aligned for best performance.

this is the easiest way:
sudo parted -a optimal /dev/sdc mkpart primary NTFS 0% 100%

via https://www.pantz.org/software/parted/parted_and_disk_alignment.html


This didn't work:

http://rainbow.chard.org/2013/01/30/how-to-align-partitions-for-best-performance-using-parted/


    Get the alignment parameters for your array (remember to replace sdb with the name of your device as seen by the kernel).

    # cat /sys/block/sdc/queue/optimal_io_size
    1048576
    # cat /sys/block/sdc/queue/minimum_io_size
    262144
    # cat /sys/block/sdc/alignment_offset
    0
    # cat /sys/block/sdc/queue/physical_block_size
    512

(33553920 + 0) / 4096

    Add optimal_io_size to alignment_offset and divide the result by physical_block_size. In my case this was (1048576 + 0) / 512 = 2048.
    This number is the sector at which the partition should start. Your new parted command should look like

    mkpart primary 2048s 100%

    The trailing ‘s’ is important: it tells parted that you’re talking about sectors, not bytes or megabytes.
    If all went well, the partition will have been created with no warnings. You can check the alignment thusly (replacing ‘1’ with the partition number if necessary):

    (parted) align-check optimal 1
    1 aligned



*2016.05.03 17:19:00
To find out what filesystem a device is formatted for, use:

sudo file -s /dev/sdd1
/dev/sdd1: DOS/MBR boot sector, code offset 0x52+2, OEM-ID "NTFS    ", sectors/cluster 8, Media descriptor 0xf8, sectors/track 32, heads 64, hidden sectors 2048, dos < 4.0 BootSector (0x80), FAT (1Y bit by descriptor); NTFS, sectors/track 32, sectors 30494719, $MFT start cluster 4, $MFTMirror start cluster 1905919, bytes/RecordSegment 2^(-1*246), clusters/index block 1, serial number 02167d0f6232476c5; contains Microsoft Windows XP/VISTA bootloader BOOTMGR

This yields more information than mount will (fuseblk is not specific enough)


*2012.10.19 06:35:35 drive wipe erase
http://www.howtogeek.com/howto/15037/use-an-ubuntu-live-cd-to-securely-wipe-your-pcs-hard-drive/

sudo apt-get install wipe

#make sure to get the right drive:
sudo fdisk -l
mount

sudo wipe /dev/sdb2
sudo wipe /dev/sdb1





*2012.03.08 11:36:25
other options:

sudo parted -a optimal /dev/sdb mklabel mac
sudo parted -a optimal /dev/sdb mklabel msdos  #is this Master Boot Record?

sudo parted -a optimal /dev/sdb mkpart primary NTFS 20.5kB 2000GB
Warning: The resulting partition is not properly aligned for best performance.
Ignore/Cancel? C


examples:

formatted with mac os X: (GUID Partition Table)
---------------------------------------------------
sudo parted -a optimal /dev/sdb print
Model: SAMSUNG HD204UI (scsi)
Disk /dev/sdb: 2000GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt

Number  Start   End     Size    File system  Name                  Flags
 1      20.5kB  210MB   210MB   fat32        EFI System Partition  boot
 2      210MB   2000GB  2000GB  hfs+         Untitled

*2009.01.27 13:27:55
#open terminal
#find your drive
mount

#see what is mounted:
#/dev/sdb1 on /media/backups type ext3 (rw,nosuid,nodev,uhelper=hal)
#/dev/sdc1 on /media/CHARLES type vfat (rw,nosuid,nodev,uhelper=hal,shortname=mix

#scan dmesg for drive devices that were detected:
dmesg

#set up the one you want to format:
sudo fdisk /dev/sdc

*2009.02.15 23:03:24
consider making swap space on the drive if there is ever a chance that it will be the only drive in a live system
(if formatting with ext3, there is a good chance of that)
much easier to spare a few gigs now, than try to create it later.

delete
new
primary
1
write

#then make the new filesystem
sudo mkfs.ext3 -L data /dev/sda1

*2010.07.18 16:11:50 ntfs
t (toggle)
7 (HPFS/NTFS)
sudo mkfs.ntfs -L DATA -f /dev/sdc1

*2011.01.30 12:43:18 hfs+
t (toggle)
af (HFS / HFS+)

format on Mac OS X using Disk Utility
Volume Format: Mac OS Extended
don't forget to change permissions for writing on linux:
sudo chmod -R 777 /media/external


*2008.10.23 16:59 macdrive7
macdrive7 looks like the software needed to mount hfs+ on windows machines.

http://www.google.com/search?client=firefox-a&rls=org.mozilla%3Aen-US%3Aofficial&channel=s&hl=en&q=windows+xp+hfs&btnG=Google+Search
windows xp hfs - Google Search
http://www.macwindows.com/disks2.html
Sharing Disks - Windows Products
http://hem.bredband.net/catacombae/hfsx.html
HFSExplorer
http://www.tomshardware.com/forum/103879-45-read-volumes-windows
How to read hfs volumes with windows xp
http://forums.macrumors.com/showthread.php?t=293781
HFS on Windows XP - Mac Forums
http://www.f.kth.se/~f96-bet/hfsutils/
HFS utilities for DOS, Windows and OS/2
http://www.mediafour.com/products/macdrive/
Mediafour | MacDrive
http://www.mediafour.com/support/macdrive/
Mediafour | MacDrive 7

*2008.02.15 10:55
hfsplus, formatted on mac, seems to load no problem in ubuntu
note [2008.02.15 11:11]
journaled is only read only on linux.  need to make it non-journaled.

##########################
# FDISK AND MKRAID
##########################

#TODO:
#fdisk these drives for raid
#mkfs -t (same as current music) /dev/md0
#with only 3 drives in system:
#sudo mdadm --assemble /dev/md0 /dev/sdc1
sudo mdadm --assemble /dev/md0 /dev/sda1 /dev/sdd1

sudo fdisk /dev/sda
sudo fdisk /dev/sdd
#use type fd - Linux raid autodetect
#mdadm --create /dev/md0 --chunk=128 --level=1 --raid-devices=2 --spare-devices=0 /dev/hde1 /dev/hdg1

sudo mdadm -C -n 2 -l 1 -x 0 /dev/sda1 /dev/sdd1

sudo mkdir /media/data
sudo mount /dev/md0 /media/data
sudo mkreiserfs /dev/md0

*2010.05.12 23:20:27
having trouble formatting drive as HFS+ after being formatted as ext3 (again... see.. [2008.10.27 14:29]
partition type set to af already
able to format it as MS-DOS (VFAT)

need to make sure that the partition size is correct
use the "Partition" option for the disk
select "2 Partitions"
allow the 1st to be the largest size allowed by configuration (might lose a few gigabytes here)
set the second one to be free space (can be reclaimed for swap later if needed)

*2013.06.05 01:30:02
sudo parted -a optimal /dev/sdb print
Model: WD My Passport 0748 (scsi)
Disk /dev/sdb: 2000GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos

Number  Start   End     Size    Type     File system  Flags
 1      1049kB  2000GB  2000GB  primary  ntfs


*2013.06.18 11:22:52
sudo parted -a optimal /dev/sdc mklabel gpt
sudo parted -a optimal /dev/sdc print

sudo parted -a optimal /dev/sdc rm 1
sudo parted -a optimal /dev/sdc rm 2

sudo parted -a optimal /dev/sdc mkpart primary NTFS 1MB 3000GB
sudo mkfs.ntfs -L PP -f /dev/sdc1