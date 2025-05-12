# MAIL

## Mail

```shell
find-qid -r "@toto\." -q deferred -p sender | sort | uniq -c | sort -rn
```

## Lock file

Bloquer la modification d'un fichier :

```shell
lsattr /etc/haproxy/haproxy.cfg
chattr +i /etc/haproxy/haproxy.cfg
# Supprimer le lock : -i
chattr -i /etc/haproxy/haproxy.cfg
```

## Replace disk with RAID

### Replace failing disk from mdadm RAID group

1. Introduction
    1.1. Main checklist for disk replacement
2. Prerequisites
3. Important notes before starting
4. Procedure

1. Introduction

This procedure describes how to replace a disk that is going to fail or that is already failed in a RAID group created by mdadm software on Linux

Usually this action is necessary when you got alert from this script `/usr/lib/nagios/plugins/check_smart`.

1.1. Main checklist for disk replacement

> here is the generic checklist for a disk replacement, all steps are described/procedured below.

- identify failing disk + take log files
- remove failing disk form RAID (+ DT the probe related mdadm)
- use a tools of cleaning on the disk for delete all files on it!
- remove from production flow
- full DT on the node
- ticket on provider
- RAID reconstruction
- check server monitoring + remove DT
- put back on production flow

2. Prerequisites

- Ensure that RAID group can support failure of 1 drive. Using command `grep -E ^md /proc/mdstat`, you can see the RAID group level (from 0 to 6). Only raid groups with a level > 0 can support a disk failure.
- Ensure that you have access to the server in rescue mode

3. Important notes before starting

> ALERT - Replace disks 1 by 1. Ensure that the new disk is working correctly before starting the replacement of another drive. Please consider time required to rebuild RAID group.

> WARNING - Disks must be erased before being removed from a server. Please, follow the appropriate procedure to erase drives before asking for their removal.

> WARNING : While this procedure can be done “online”, with OS still running, it is advise to do it “cold” by restarting server in rescue mode. 2 main reasons for this:

- Often, your server will need to be rebooted anyway by the hosting provider in order to replace disks.
- erase disk

4. Procedure

When possible, restart the server in rescue mode. This is advised (see important notes above).

1. Identify disks that is failing. You can use `smartcl -a` command to see errors on disks:

```shell
smartctl -a /dev/sda | grep FAIL
```

Output :

```shell
173 Wear_Leveling_Count     0x0033   001   001   005    Pre-fail  Always   FAILING_NOW 1758
```

1. Identify disk model and serial number using smartctl -a command:

```shell
smartctl -a /dev/sda | grep -m1 -A 15 "INFORMATION"
```

Output :

```shell
=== START OF INFORMATION SECTION ===
Model Family:     Samsung based SSDs
Device Model:     SAMSUNG MZ7LN1T0HAJQ-00000
Serial Number:    S3TXNX0K900518
LU WWN Device Id: 5 002538 d42cf6fd1
Firmware Version: MVT0300Q
User Capacity:    1 024 209 543 168 bytes [1,02 TB]
Sector Size:      512 bytes logical/physical
Rotation Rate:    Solid State Device
Form Factor:      2.5 inches
Device is:        In smartctl database [for details use: -P show]
ATA Version is:   ACS-2, ATA8-ACS T13/1699-D revision 4c
SATA Version is:  SATA 3.1, 6.0 Gb/s (current: 6.0 Gb/s)
Local Time is:    Tue May 16 12:10:34 2023 CEST
SMART support is: Available - device has SMART capability.
SMART support is: Enabled
```

:light_bulb_on: Tip:

run those commands to extract smartctl result in a file you can attach to your ticket (good chance is the provider will ask the details)

```shell
device='/dev/sda'
logfile=/tmp/smartctl-$(hostname -s)-$(echo $device | tr '/' '_')-$(date +%F-%H-%M-%S).log
smartctl -a $device > $logfile
```

Add the smartctl log file to ticket for reference

Check current status of RAID groups:

this should show `[UUU]` or `[UU]` beside each volume and raid should be RAID1 or RAID5

```shell
cat /proc/mdstat
```

Output:
```shell
Personalities : [raid6] [raid5] [raid4] [linear] [multipath] [raid0] [raid1] [raid10]
md1 : active raid5 sdb3[1] sdc3[2] sda3[3]
      1999093760 blocks super 1.2 level 5, 512k chunk, algorithm 2 [3/3] [UUU]
      bitmap: 4/8 pages [16KB], 65536KB chunk

md0 : active raid5 sdb2[1] sdc2[2] sda2[3]
      1012736 blocks super 1.0 level 5, 512k chunk, algorithm 2 [3/3] [UUU]

unused devices: <none>
```

Identify which disk partition is managed by each RAID partition

```shell
lsblk -p
```

Remove this disk from RAID groups it belong to (here, we’ll remove disk `/dev/sda` which belongs to 2 RAID groups):

```shell
# mdadm --manage /dev/md0 --fail /dev/sda2 --remove /dev/sda2
mdadm: set /dev/sdb2 faulty in /dev/md0
mdadm: hot removed /dev/sdb2 from /dev/md0

# mdadm --manage /dev/md1 --fail /dev/sda3 --remove /dev/sda3
mdadm: set /dev/sdb3 faulty in /dev/md1
mdadm: hot removed /dev/sdb3 from /dev/md1
```

Ensure that RAID group does not contains disk anymore. Status should by `U_` or `U_U` depending on number of disks in RAID group (`U` means that disk is OK, while `_` means that disk is not OK).

```shell
cat /proc/mdstat
```

Output :

```shell
Personalities : [raid6] [raid5] [raid4] [linear] [multipath] [raid0] [raid1] [raid10]
md1 : active raid5 sdc3[2] sda3[3]
      1999093760 blocks super 1.2 level 5, 512k chunk, algorithm 2 [3/2] [U_U]
      bitmap: 7/8 pages [28KB], 65536KB chunk

md0 : active raid5 sdc2[2] sda2[3]
      1012736 blocks super 1.0 level 5, 512k chunk, algorithm 2 [3/2] [U_U]

unused devices: <none>
```

Erase disk using erasure procedure:

- Ask for disk replacement to the hosting provider.
- You’ll need to provide Serial number of the disk and sometimes even the full smartctl log (see tip above)
- Usually it will be requested to do the operation offline. It is advised to reboot server in rescue mode for such cases.
- After a disk has been replaced, you have to resume the RAID.

!> Warning: if RAID is considered as inactive (this happen if disk has not been properly removed from RAID group before its replacement), you have to reactivate it first using these commands (adapt RAID device `/dev/md1` and disk partitions (`/dev/sdb3`, `/dev/sdc3` to your use case):

```shell
# mdadm --stop /dev/md1
# mdadm -A --force /dev/md1 /dev/sdb3 /dev/sdc3
```

In our exemple, working disks are `/dev/sdb` and `/dev/sdc`, while replaced disk is `/dev/sda`. We will recreate partitions of disk `/dev/sda`, from disk `/dev/sdb`.

```shell
sfdisk -d /dev/sdb | sfdisk -f /dev/sda
```

Checking that no-one is using this disk right now ... OK

```shell
Disk /dev/sda: 953,89 GiB, 1024209543168 bytes, 2000409264 sectors
Disk model: SAMSUNG MZ7LN1T0
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Created a new GPT disklabel (GUID: 2AE3858B-8CAB-4252-BA8E-A43695DA7E25).
/dev/sda1: Created a new partition 1 of type 'BIOS boot' and of size 16 MiB.
/dev/sda2: Created a new partition 2 of type 'Linux RAID' and of size 495 MiB.
/dev/sda3: Created a new partition 3 of type 'Linux RAID' and of size 476.4 GiB.
/dev/sda4: Done.

New situation:
Disklabel type: gpt
Disk identifier: 2AE3858B-8CAB-4252-BA8E-A43695DA7E25

Device       Start        End   Sectors   Size Type
/dev/sda1     2048      34815     32768    16M BIOS boot
/dev/sda2    34816    1048576   1013761   495M Linux RAID
/dev/sda3  1050624 1000215182 999164559 476.4G Linux RAID

The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

```shell
lsblk
```
Output:

```shell
NAME                MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sda                   8:0    0   477G  0 disk
├─sda1                8:1    0    16M  0 part
├─sda2                8:2    0   495M  0 part
│ └─md0               9:0    0   495M  0 raid1 /boot
└─sda3                8:3    0 476.4G  0 part
  └─md1               9:1    0 476.3G  0 raid1
    └─lvm_crypt     253:0    0 476.3G  0 crypt
      ├─system-swap 253:1    0     1G  0 lvm   [SWAP]
      └─system-root 253:2    0 475.3G  0 lvm   /
sdb                   8:16   0   477G  0 disk
├─sdb1                8:17   0    16M  0 part
├─sdb2                8:18   0   495M  0 part
└─sdb3                8:19   0 476.4G  0 part
```

Now, resume all raid partitions:

```shell
# mdadm --manage /dev/md0 --add /dev/sdb2
mdadm: added /dev/sdb2

# mdadm --manage /dev/md1 --add /dev/sdb3
mdadm: added /dev/sdb3
```


```shell
cat /proc/mdstat
```

Output:

```shell
Personalities : [raid6] [raid5] [raid4] [linear] [multipath] [raid0] [raid1] [raid10]
md126 : active raid5 sdb3[4] sda3[3] sdc3[2]
      1999093760 blocks super 1.2 level 5, 512k chunk, algorithm 2 [3/2] [U_U]
      [>....................]  recovery =  0.0% (401436/999546880) finish=124.4min speed=133812K/sec
      bitmap: 8/8 pages [32KB], 65536KB chunk

md127 : active raid5 sdb2[4] sda2[3] sdc2[2]
      1012736 blocks super 1.0 level 5, 512k chunk, algorithm 2 [3/3] [UUU]

unused devices: <none>
```

Finaly, copy content of `sdb1` into `sda1` (required for BIOS boot):

```shell
dd if=/dev/sdb1 of=/dev/sda1 bs=1M
```

Output:
```shell
16+0 records in
16+0 records out
16777216 bytes (17 MB, 16 MiB) copied, 0.0988978 s, 170 MB/s
```

And last step, execute grub-install on all disks

You can now reboot server in normal mode (if needed). Don’t forget to re-enable it in Mandos before.
