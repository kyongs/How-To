1. Check the device name to mount

```
$ sudo fdisk -l

Disk /dev/sdb: 238.5 GiB, 256060514304 bytes, 500118192 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xfe623855

Device     Boot Start       End   Sectors   Size Id Type
/dev/sdb1        2048 500118191 500116144 238.5G 83 Linux

```

2. If needed, create partition.
   (Enter `n` to create new partition, then `w` to write the changes you've made to disk.)

```
$ sudo fdisk /dev/sdb1

Welcome to fdisk (util-linux 2.27.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): n
...
```

3. Create a filesystem (ex. EXT4) on the partition.

```
vldb@vldb:~$ sudo mkfs.ext4 /dev/sdb1
mke2fs 1.44.1 (24-Mar-2018)
/dev/sdb1 contains a ext4 file system
	last mounted on /home/vldbjt/heat_prediction/my/log on Wed Jul 21 01:57:13 2021
Proceed anyway? (y,N) y
Discarding device blocks: done
Creating filesystem with 62514518 4k blocks and 15630336 inodes
Filesystem UUID: 42d82415-9fff-454e-91ae-574696413908
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
	4096000, 7962624, 11239424, 20480000, 23887872

Allocating group tables: done
Writing inode tables: done
Creating journal (262144 blocks): done
Writing superblocks and filesystem accounting information: done

vldb@vldb:~$

```

4. Mount data device. (Below is the example.)

```
$ mkdir test_data
$ sudo mount /dev/nvme0n1p1 test_data
$ sudo chown -R yourUsername:yourUsername test_data
```

5. Mount log device.

```
$ mkdir test_log
$ sudo mount /dev/sdb1 -o nobarrier test_log
$ sudo chown -R yourUsername:yourUsername test_log
```

6. Check the mounted devices.

```
$ df -h

/dev/sdb1       234G   61M  222G   1% /home/vldb/log-data
```
