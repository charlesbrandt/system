*2019.07.08
https://help.ubuntu.com/community/DataRecovery
http://www.system-rescue-cd.org/System-tools/

Starting with TestDisk

*2014.03.31 07:18:25
on HFS+, mac formatted drives...
If drive has been synchronized and meta folders have been deleted / corrupted, when first connecting drive to an actual OS X system, give it time to rebuild indexes before attempting a First Aid / Repair with Disk Utility.  I have seen a perfectly fine drive report "Cannot recover, back up all data".  Then I let the indexer run, and a few hours later no errors were reported with Disk Utility.  Give it time. 

*2012.10.06 16:57:15 
can't seem to find notes on this process

hopefully your backup processes are good enough that if a drive fails, or if the filesystem becomes corrupt enough, that you can easily just recover from backup, rather than try to repair the disk.

But it happens... that data that isn't quite important enough to worry about, but is worth a little effort to try to recover.  

*2012.10.06 17:00:05 
in the past have used:
Get Data Back 
software on Windows (usually seems to be NTFS filesystems that go bad most often)

*2012.10.06 17:20:17 
but sometimes the hardware does fail
and in that case, check if the product is still under warranty
most drives have a 3 year warranty, and many drives fail within that time, if they're going to fail.








*2014.03.26 10:32:54
recovering RAID

haven't been using software level RAID systems as much these days.  Recovery is not straight forward.

Started looking into it here, then abandoned the effort:

https://www.google.com/search?q=mount+one+linux+raid+drive&oq=mount+one+linux+raid+drive&aqs=chrome..69i57.12558j0j1&sourceid=chrome&espv=210&es_sm=91&ie=UTF-8
https://bbs.archlinux.org/viewtopic.php?id=156477
http://ubuntuforums.org/showthread.php?t=1424978

requires mdadm tools.

https://wiki.archlinux.org/index.php/RAID#Mounting_from_a_Live_CD


then:
assemble the raid:

mdadm --assemble /dev/<disk1> /dev/<disk2> /dev/<disk3> /dev/<disk4>

mdadm -E -s

*2014.04.22 18:48:31 disk_warrior diskwarrior alsoft
considering purchasing a license to Disk Warrior

I know I considered this in the past.  Was looking for a license, but I think I opted against the purchase.  $100... pretty pricey.

https://www.google.com/search?q=invalid+map+node+linkage&oq=invalid+map+node+linkage&aqs=chrome..69i57.4462j0j1&sourceid=chrome&espv=210&es_sm=91&ie=UTF-8
https://discussions.apple.com/thread/141900
http://alsoft.com/DiskWarrior/index.html
http://alsoft.com/Buy/index.html

