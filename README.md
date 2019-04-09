# ZFS useful commands and examples


## [1 Create a simple disk using a single file](https://github.com/robertoarcomano/zfs/blob/master/README.md#1-create-a-simple-disk-using-a-single-file-1)
## [2 Create a RAID1 array using 2 files](https://github.com/robertoarcomano/zfs#2-create-a-raid1-array-using-2-files)
## [3 Create a Device and then attach to it a new Device to create RAID1](https://github.com/robertoarcomano/zfs#3-create-a-device-and-then-attach-to-it-a-new-device-to-create-raid1)


## 1 Create a simple disk using a single file 
#### 1.1 Create a dir and enter it
```
mkdir zfs
cd zfs
```
#### 1.2 Create a file
```
dd if=/dev/zero of=1G bs=1G count=1
```
#### 1.3 Create mount point
```
mkdir -p /media/disk1
```
#### 1.4 Create ZFS file system with file device and set mount point
```
zpool create disk1 /home/berto/zfs/1G -m /media/disk1
```
#### 1.5 Show the mounted disk
```
# df -hT /media/disk1/
Filesystem     Type  Size  Used Avail Use% Mounted on
disk1          zfs   880M     0  880M   0% /media/disk1
```
#### 1.6 Show the ZFS device
```
# zpool list disk1 -v
NAME   SIZE  ALLOC   FREE  EXPANDSZ   FRAG    CAP  DEDUP  HEALTH  ALTROOT
disk1  1008M   120K  1008M         -     0%     0%  1.00x  ONLINE  -
  /home/berto/zfs/1G  1008M   120K  1008M         -     0%     0%
```

## 2 Create a RAID1 array using 2 files
#### 2.1 Create 2 files (RAID_DISK1 and RAID_DISK2)
```
dd if=/dev/zero of=RAID_DISK1 bs=1G count=1
dd if=/dev/zero of=RAID_DISK2 bs=1G count=1
```
#### 2.2 Create mount point
```
mkdir -p /media/raid_disk
```
#### 2.3 Create ZFS with RAID with 2 files device and set mount point
```
zpool create raid_disk mirror /home/berto/zfs/RAID_DISK1 /home/berto/zfs/RAID_DISK2 -m /media/raid_disk
```
#### 2.4 Show the mounted disk
```
# df -hT /media/raid_disk
Filesystem     Type  Size  Used Avail Use% Mounted on
raid_disk      zfs   880M     0  880M   0% /media/raid_disk
```
#### 2.5 Show the ZFS device
```
# zpool list raid_disk -v
NAME   SIZE  ALLOC   FREE  EXPANDSZ   FRAG    CAP  DEDUP  HEALTH  ALTROOT
raid_disk  1008M   120K  1008M         -     0%     0%  1.00x  ONLINE  -
  mirror  1008M   120K  1008M         -     0%     0%
    /home/berto/zfs/RAID_DISK1      -      -      -         -      -      -
    /home/berto/zfs/RAID_DISK2      -      -      -         -      -      -
```

## 3 Create a Device and then attach to it a new Device to create RAID1
#### 2.1 Create 2 files (RAID_DISK1 and RAID_DISK2)
```
dd if=/dev/zero of=RAID_DISK1 bs=1G count=1
dd if=/dev/zero of=RAID_DISK2 bs=1G count=1
```
#### 3.2 Create mount point
```
mkdir -p /media/raid_disk
```
#### 3.3 Create ZFS with RAID with 1 file device and set mount point
```
zpool create raid_disk /home/berto/zfs/RAID_DISK1 -m /media/raid_disk
```
#### 3.4 Attach new file device to create a RAID1
```
zpool attach raid_disk /home/berto/zfs/RAID_DISK1 /home/berto/zfs/RAID_DISK2
```
#### 3.5 Show the mounted disk
```
# df -hT /media/raid_disk
Filesystem     Type  Size  Used Avail Use% Mounted on
raid_disk         zfs   880M  128K  880M   1% /media/raid_disk
```
#### 3.6 Show the ZFS device
```
# zpool list raid_disk -v
NAME   SIZE  ALLOC   FREE  EXPANDSZ   FRAG    CAP  DEDUP  HEALTH  ALTROOT
raid_disk  1008M   710K  1007M         -     0%     0%  1.00x  ONLINE  -
  mirror  1008M   710K  1007M         -     0%     0%
    /home/berto/zfs/RAID_DISK1      -      -      -         -      -      -
    /home/berto/zfs/RAID_DISK2      -      -      -         -      -      -

```
