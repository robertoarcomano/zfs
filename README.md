# zfs
## ZFS useful commands and examples

#### Create a dir and enter it
```
mkdir zfs
cd zfs
```
#### Initialize a file
```
dd if=/dev/zero of=1G bs=1G count=1
```
#### Create mount point
```
mkdir -p /media/disk1
```
#### Create ZFS file system with file device and set mount point
```
zpool create disk1 /home/berto/zfs/1G -m /media/disk1
```
#### Show the result
```
# df -hT /media/disk1/
Filesystem     Type  Size  Used Avail Use% Mounted on
disk1          zfs   880M     0  880M   0% /media/disk1
```

