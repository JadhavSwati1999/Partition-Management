# Partition-Management
## Partition management

A partition divides large parts into small parts for backup data in different partitions.

### There are two types of partition

1. SSD (Solid State Drive)
2. HDD (Hard Disk Drive)

## Filesystem

Used to store data in a structured manner.

- ext4 is the most used filesystem in linux.
- xfs is also a filesystem.

## Mounting

Managing process of adding a new file system into the existing directory is called mounting & the directory is called the mounting point.

## There are two schemes of partition

1. mbr (master boot record)
2. GPT (GUID global user identification)

| Parameter | MBR | GPT |
| --- | --- | --- |
| Definition | MBR is an older partitioning scheme that divides the disk into two types of partitions – primary and extended | GPT is a modern partitioning scheme that theoretically doesn’t have any limitations on a number of partitions but is limited only by the OS. |
| Boot Loader used | Master Boot Program stored at the first sector of the disk | A special partition called – EFI, especially allocated for loading the OS. |
| Addressing Scheme | It uses a 32-bit addressing scheme limiting the storage capacity to at most 2TB | It uses 64-bit addressing capable of dealing with 9.4 zeta bytes of storage capacity |
| Compatibility | It is commonly used with Legacy BIOS based systems | It is commonly used with modern UEFI-based systems |
| Partition Limitation | It supports a maximum of 4 primary partitions | Upto 128 partitions |
| Data Integrity | It doesn’t support an error detection mechanism and is less secure | It supports the CRC-32 error detection mechanism |
| Data Recovery | No built-in mechanism for data recovery in this scheme | GPT stores the identical copy at the end of the disk of the partition table which is allocated in the beginning of the disk. It can be used in case of data corruption for recovery. |

## Commands

1. lsblk  - to list all partition
2. blkid  - to check whether the filesystem is attached or not
3. df -h  - to check filesystem size & mount
4. fdisk  -  to create partition pre-install partitioning utility

## GPT

If you want to add a new storage and if system supports GPT protocol you can use cfdisk. 

## Steps to create partition

1. Create storage block device
2. Create partition
3. We have to format the file system
4. Create the data and store inside the directory or files
5. Mount the data

## Q How to create storage block device ?

If the system is MBR then we have to create block device **manually**

If the system is GPT then we use **cfdisk /dev/sdb**

## Explain the lsblk output or fields ?

1. **Name** - Storage device name
2. **Major** - block device so that kernel can communicate with os.
3. **Minor -** differentiate between block device partition.
4. **RM** - read minor number
5. **Size** - data ssize
6. **Ro** - read only[(o → default(R - w)(1 → readonly))] (data accessibility)
7. **Type** - 
8. **Mountpoint** - Where the data is stored that location

- **Swap memory -** When RAM is insufficient to manage work performance then we can use system storage as a RAM that's called as swap or virtual memory. (temporary storage)

 mkswap /dev/sdb3  - to create a swap

lsblk -f /dev/sdb   - to view the swap

swapon /dev/sdb3  - to get in active state

swapoff  /dev/sdb3  - to deactivate.

## Q How to create a swap file ?

dd if(input file) = /dev/zero of(output file) =swapfile

bs =1M count=128

mkswap /swap file

swapon /swapfile

swapon - -show or lsblk -f /dev/sdb 

### To make permanently changes make changes inside the fstab file.

mkfs.ext4 /dev/sd1     — to format a filesystem

lsblk -f  /dev/sdb        —to check filesystem of a particular partition

### partprobe : to update the partition table

## LVM(Logical Volume Management)

Use to combine the partitions 



## Steps

1. We have to add disk  eg: sdc
2. We have to create a physical portion  eg: sdc1
3. Create physical volume  eg: /dev/sdc
4. Create volume group  eg: my-volume
5. Create logical volume  
6. Format filesystem on that logical volume
7. Create data (this is optional) if data is present you can use that
8. Mount

## Commands

fdisk -f  /dev/sdc

lsblk -f  /dev/sdc

pvcreate  /dev/sdc

pvs - short info

pvdisplay - detailed info

vgcreate groupname eg:vgcreate myvolume

vgs - how many volume group our there 

vgdisplay - detailed info

lvcreate -l +2G   -n new_vol myvolume

vgextend  length group name  /dev/sdb3

lvcreate  -l +2G -n  new_vol myvolume 

## Format filesystem

mkfs.xfs  /dev/my_vol/new_lim

mkdir  /new_dir

mount  /dev/my_vol/new_lum/new_dir

df-ht  to see filesystem full (overall)

lvextend  -n  /dev/my_vol/new_lum  -l +5G

lvresize  -n   /dev/my_vol/new_lum  -l  -2G

pvremove  /dev/sdc2  -to remove

vgreduce   my_vol (groupname)

```jsx
**Note:
If we try to add logical volume which takes more GB than the 
volume group, then we have to extend the volume group
1 have to add a storage device
2 add a partition in the existing storage device**
 
```
