# mdadm-notes

## 初期設定

```
sudo mdadm --detail --scan > /etc/mdstat.conf
```

ex.
```
ARRAY /dev/md/0 metadata=1.2 name=raspberrypi:0 UUID=f55fd29a:81e96f88:d5667885:830cb7d9
```

### Add disk
```
mdadm --add /dev/md0 /dev/sdb
```

## 状態確認

### mdstat 確認
```
cat /proc/mdstat
```
ex.
```
Personalities : [raid6] [raid5] [raid4]
md0 : active raid5 sde1[1] sdf1[2] sdg1[4] sdd1[0]
      8790402048 blocks super 1.2 level 5, 512k chunk, algorithm 2 [4/4] [UUUU]
      bitmap: 0/22 pages [0KB], 65536KB chunk

unused devices: <none>
```

```
mdadm --detail /dev/md0
```
ex.

```
/dev/md0:
           Version : 1.2
     Creation Time : Sat Aug 26 13:05:18 2017
        Raid Level : raid5
        Array Size : 8790402048 (8383.18 GiB 9001.37 GB)
     Used Dev Size : 2930134016 (2794.39 GiB 3000.46 GB)
      Raid Devices : 4
     Total Devices : 4
       Persistence : Superblock is persistent

     Intent Bitmap : Internal

       Update Time : Sat Feb  6 19:06:05 2021
             State : clean
    Active Devices : 4
   Working Devices : 4
    Failed Devices : 0
     Spare Devices : 0

            Layout : left-symmetric
        Chunk Size : 512K

Consistency Policy : bitmap

              Name : raspberrypi:0
              UUID : f55fd29a:81e96f88:d5667885:830cb7d9
            Events : 296377

    Number   Major   Minor   RaidDevice State
       0       8       49        0      active sync   /dev/sdd1
       1       8       65        1      active sync   /dev/sde1
       2       8       81        2      active sync   /dev/sdf1
       4       8       97        3      active sync   /dev/sdg1
```

### smartctl
```
sudo yum install smartmontools
```
```
sudo smartctl --scan
```

ex.
```
/dev/sda -d scsi # /dev/sda, SCSI device
/dev/sdb -d usbprolific # /dev/sdb [USB Prolific], ATA device
/dev/sdc -d usbprolific # /dev/sdc [USB Prolific], ATA device
```

### 
```
sudo smartctl -i /dev/sda
```

### 
```
sudo smartctl -A /dev/sda
```

#### test disks
```
sudo smartctl -t short /dev/sda
sudo smartctl -t long /dev/sda
```

The result can be displayed by this following command.
```
sudo smartctl -a /dev/sda
```

## Replace disk
If you want to change a disk, you need to stop the disk before removing.
```
mdadm --fail /dev/md126 /dev/sda
```

Then you can remove it.
```
mdadm --remove /dev/md126 /dev/sda
```
```
mdadm /dev/md0 -r /dev/sdc1
```

```
mdadm -D /xxx
```


