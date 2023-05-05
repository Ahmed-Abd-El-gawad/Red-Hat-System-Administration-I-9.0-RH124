# Chapter 15. Access Linux File Systems

| content |
| --- |
| [15.1 Identify File Systems and Devices](#15.1) |
| [15.3 Mount and Unmount File Systems](#15.3) |
| [15.5 Locate Files on the System](#15.5) |


<a name="15.1"></a>
## 15.1 Identify File Systems and Devices

| Content |
| --- |
| [Storage Management Concepts](#Storage_Management_Concepts) |
| [File Systems and Mount Points](#File_Systems_and_Mount_Points) |
| [File Systems, Storage, and Block Devices](#File_Systems_Storage_and_Block_Devices) |
| [Disk Partitions](#Disk_Partitions) |
| [Logical Volumes](#Logical_Volumes) |
| [Examine File Systems](#Examine_File_Systems) |

<a name="Storage_Management_Concepts"></a>
* Storage Management Concepts
  * Red Hat Enterprise Linux (**RHEL**) uses the Extents File System (***XFS***) as the default local file system. 
  * RHEL supports the Extended File System (***ext4***) file system for managing local files. 
  * Starting with RHEL 9, the **exFAT** file system is supported for removable media use. In an enterprise server cluster, shared disks use the Global File System 2 (GFS2) file system to manage concurrent multi-node access.

<a name="File_Systems_and_Mount_Points"></a>
* File Systems and Mount Points
  * Access the contents of a file system by mounting it on an empty directory. This directory is called a mount point. When mounted, use the ls command to list the contents of that directory. Many file systems are automatically mounted when the system boots.
  * A mount point is a slightly different concept than a Microsoft Windows drive letter, where each file system is a separate entity. Mount points allow multiple file system devices to be available in a single tree structure. This is similar to NTFS mounted folders in Microsoft Windows.

<a name="File_Systems_Storage_and_Block_Devices"></a>
* File Systems, Storage, and Block Devices
  * A block device is a file that provides low-level access to storage devices. A block device must be optionally partitioned, and a file system created before the device can be mounted.
  * The ```/dev``` directory stores block device files, which RHEL creates automatically for all devices. In RHEL 9, the first detected SATA, SAS, SCSI, or USB hard drive is called the ```/dev/sda``` device; the second is the ```/dev/sdb``` device; and so on. These names represent the entire hard drive.

    | Type of device | Device naming pattern |
    | --- | --- |
    | SATA/SAS/USB-attached storage (SCSI driver) | ```/dev/sda```, ```/dev/sdb```, ```/dev/sdc```, … |
    | ```virtio-blk``` paravirtualized storage (VMs) | ```/dev/vda```, ```/dev/vdb```, ```/dev/vdc```,… |
    | ```virtio-scsi``` paravirtualized storage (VMs) | ```/dev/sda```, ```/dev/sdb```, ```/dev/sdc```, … |
    | NVMe-attached storage (SSDs) | ```/dev/nvme0```, ```/dev/nvme1```, … |
    | SD/MMC/eMMC storage (SD cards) | ```/dev/mmcblk0```, ```/dev/mmcblk1```, … |

<a name="Disk_Partitions"></a>
* Disk Partitions
  * With partitions, you can compartmentalize a disk: the various partitions might be formatted with different file systems or be used for other purposes. For example, one partition might contain user home directories, while another partition might contain system data and logs. Even when the home directory partition is loaded with data, the system partition might still have available space.
  * Partitions are block devices in their own right. For example, on the first SATA-attached storage, the first partition is the ```/dev/sda1``` disk. The second partition of the same storage is the ```/dev/sda2``` disk. The third partition on the third SATA-attached storage device is the ```/dev/sdc3``` disk, and so on. Paravirtualized storage devices have a similar naming system. For example, the first partition on the first storage device is the ```/dev/vda1``` disk. The second partition of the second storage device is the ```/dev/vdb2``` disk, and so on.
  * An NVMe-attached SSD device names its partitions differently from a SATA-attached device. For NVMe storage devices, the nvmeX part of the name refers to the device, the nY part refers to the namespace, and the ```pZ``` part refers to the partition. For example, the first partition for the first namespace on the first disk is the ```/dev/nvme0n1p1``` partition. The third partition for the first namespace on the second disk is the ```/dev/nvme1n1p3``` partition, and so on.
  * SD or MMC cards can sometimes have a similar naming system to the SATA devices (```/dev/sdN```), but it is not always the case. In some cases, SD or MMC cards might have names such as ```/dev/mmcblk0p1```, where the mmcblkX part of the name refers to the storage device and the pY part of the name refers to the partition number on that device.
  * An extended listing of the /dev/sda1 device file on the host machine reveals the b file type, which stands for a block device:
    ```console
    [user@host ~]$ ls -l /dev/sda1
    brw-rw----. 1 root disk 8, 1 Feb 22 08:00 /dev/sda1
    ```

<a name="Logical_Volumes"></a>
* Logical Volumes
  * Another way of organizing disks and partitions is with ***Logical Volume Management (LVM)***. With LVM, it is possible to aggregate block devices into a volume group. Disk space in the volume group is separated into logical volumes, which are the functional equivalent of a partition on a physical disk.
* The LVM system assigns names to volume groups and logical volumes on their creation. LVM creates a directory in the ```/dev``` directory that matches the group name, and creates a symbolic link within that new directory with the same name as the logical volume. That logical volume file is then available to be mounted. For example, when a myvg volume group and the ```mylv``` logical volume are present, the full path to the logical volume is the ```/dev/myvg/mylv``` file.

<a name="Examine_File_Systems"></a>
* Examine File Systems
  * Use the ```df``` command to display an overview of local and remote file-system devices, which includes the total disk space, used disk space, free disk space, and the percentage of the entire disk space.
    ```console
    [user@host ~]$ df
    Filesystem     1K-blocks    Used Available Use% Mounted on
    devtmpfs          912584       0    912584   0% /dev
    tmpfs             936516       0    936516   0% /dev/shm
    tmpfs             936516   16812    919704   2% /run
    tmpfs             936516       0    936516   0% /sys/fs/cgroup
    /dev/vda3        8377344 1411332   6966012  17% /
    /dev/vda1        1038336  169896    868440  17% /boot
    tmpfs             187300       0    187300   0% /run/user/1000
    ```
  * The partitioning shows that two physical file systems are mounted on the ```/``` and ```/boot``` directories that commonly exist on virtual machines. The ```tmpfs``` and ```devtmpfs``` devices are file systems in system memory. All files that are written to the tmpfs or devtmpfs file system disappear after a system reboot.
  * The ```df``` command ```-h``` or ```-H``` options are human-readable options to improve the readability of the output sizes. The ```-h``` option reports in KiB (210), MiB (220), or GiB (230), while the ```-H``` option reports in SI units: KB (103), MB (106), or GB (109). Hard drive manufacturers usually use SI units when advertising their products.
    ```console
    [user@host ~]$ df -h
    Filesystem      Size  Used Avail Use% Mounted on
    devtmpfs        892M     0  892M   0% /dev
    tmpfs           915M     0  915M   0% /dev/shm
    tmpfs           915M   17M  899M   2% /run
    tmpfs           915M     0  915M   0% /sys/fs/cgroup
    /dev/vda3       8.0G  1.4G  6.7G  17% /
    /dev/vda1      1014M  166M  849M  17% /boot
    tmpfs           183M     0  183M   0% /run/user/1000
    ```
  * Use the ```du``` command for more detailed information about a specific directory tree space. The ```du``` command ```-h``` and ```-H``` options convert the output to a human-readable format. The ```du``` command shows the size of all files in the current directory tree recursively.
    ```console
    [root@host ~]# du /usr/share
    ...output omitted...
    176 /usr/share/smartmontools
    184 /usr/share/nano
    8 /usr/share/cmake/bash-completion
    8 /usr/share/cmake
    356676  /usr/share
    ```
    ```console
    [root@host ~]# du -h /usr/share
    ...output omitted...
    176K  /usr/share/smartmontools
    184K  /usr/share/nano
    8.0K  /usr/share/cmake/bash-completion
    8.0K  /usr/share/cmake
    369M  /usr/share
    ```


<a name="15.3"></a>
## 15.3 Mount and Unmount File Systems

| Content |
| --- |
| [Mount File Systems Manually](#Mount_File_Systems_Manually) |
| [Identify a Block Device](#Identify_a_Block_Device) |
| [Mount File System with the Partition Name](#Mount_File_System_with_the_Partition_Name) |
| [Mount File System with Partition UUID](#Mount_File_System_with_Partition_UUID) |
| [Automatically Mount Removable Storage Devices](#Automatically_Mount_Removable_Storage_Devices) |
| [Unmount File Systems](#Unmount_File_Systems) |

<a name="Mount_File_Systems_Manually"></a>
* Mount File Systems Manually
  * To access the file system on a removable storage device, you must mount the storage device. With the mount command, the ```root``` user can mount a file system manually. The first argument of the ```mount``` command specifies the file system to mount. The second argument specifies the directory as the mount point in the file-system hierarchy.
  * You can mount the file system in one of the following ways with the ```mount``` command:
    * With the device file name in the ```/dev directory```.
    * With the UUID, a universally unique identifier of the device.

<a name="Identify_a_Block_Device"></a>
* Identify a Block Device
  * A hot-pluggable storage device, whether a hard disk drive (HDD) or a solid-state device (SSD) in a server, or alternatively a USB storage device, might be plugged each time into a different port on a system. Use the ```lsblk``` command to list the details of a specified block device or of all the available devices.
    ```console
    [root@host ~]# lsblk
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
    vda    252:0    0   10G  0 disk
    ├─vda1 252:1    0    1M  0 part
    ├─vda2 252:2    0  200M  0 part /boot/efi
    ├─vda3 252:3    0  500M  0 part /boot
    └─vda4 252:4    0  9.3G  0 part /
    vdb    252:16   0    5G  0 disk
    vdc    252:32   0    5G  0 disk
    vdd    252:48   0    5G  0 disk
    ```
  * The partition size helps to identify the device when the partition name is unknown. For example, considering the previous output, if the size of the identified partition is 9.3 GB, then mount the ```/dev/vda4``` partition.

<a name="Mount_File_System_with_the_Partition_Name"></a>
* Mount File System with the Partition Name
  * The following example mounts the ```/dev/vda4``` partition on the ```/mnt/data``` mount point.
    ```console
    [root@host ~]# mount /dev/vda4 /mnt/data
    ```
    The mount point directory must exist before mounting the file system. The /mnt directory exists for use as a temporary mount point.

  * Device detection order and storage device naming can change when devices are added or removed on a system. It is recommended to use an unchanging device identifier for mount file systems consistently.

<a name="Mount_File_System_with_Partition_UUID"></a>
* Mount File System with Partition UUID
  * One stable identifier that is associated with a file system is its universally unique identifier (UUID). This UUID is stored in the file system superblock and remains the same until the file system is re-created.
  * The ```lsblk -fp``` command lists the full path of the device, the UUIDs and mount points, and the partition's file-system type. The mount point is blank when the file system is not mounted.
    ```console
    [root@host ~]# lsblk -fp
    NAME        FSTYPE FSVER LABEL UUID                   FSAVAIL FSUSE% MOUNTPOINTS
    /dev/vda
    ├─/dev/vda1
    ├─/dev/vda2 vfat   FAT16       7B77-95E7              192.3M     4% /boot/efi
    ├─/dev/vda3 xfs          boot  2d67e6d0-...-1f091bf1  334.9M    32% /boot
    └─/dev/vda4 xfs          root  efd314d0-...-ae98f652    7.7G    18% /
    /dev/vdb
    /dev/vdc
    /dev/vdd
    ```
  * Mount the file system by the file-system UUID.
    ```console
    [root@host ~]# mount UUID="efd314d0-b56e-45db-bbb3-3f32ae98f652" /mnt/data
    ```
    
<a name="Automatically_Mount_Removable_Storage_Devices"></a>
* Automatically Mount Removable Storage Devices
  * When using the graphical desktop environment, the system automatically mounts removable storage media when the media presence is detected.
  * The removable storage device mounts at the ```/run/media/USERNAME/LABEL``` location. USERNAME is the name of the user that is logged in to the graphical environment. LABEL is an identifier, which is typically the label on the storage media.
  * To safely detach a removable device, manually unmount all file systems on the device first.

<a name="Unmount_File_Systems"></a>
* Unmount File Systems
  * System shutdown and reboot procedures unmount all file systems automatically. All file-system data that is flushed to the storage device, to ensure file system data integrity.
  * The ```umount``` command uses the mount point as an argument to unmount a file system.
    ```console
    [root@host ~]# umount /mnt/data
    ```
  * Unmounting is not possible when the mounted file system is in use. All processes must stop accessing data under the mount point for the ```umount``` command to succeed.
    
    In the following example, the ```umount``` command fails because the shell uses the ```/mnt/data``` directory as its current working directory, and thus generates an error message.
    ```console
    [root@host ~]# cd /mnt/data
    [root@host data]# umount /mnt/data
    umount: /mnt/data: target is busy.
    ```
  * The ```lsof``` command lists all open files and the processes that are accessing the file system. The list helps to identify which processes are preventing the file system from successfully unmounting.
    ```console
    [root@host data]# lsof /mnt/data
    COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
    bash    1593 root  cwd    DIR 253,17        6  128 /mnt/data
    lsof    2532 root  cwd    DIR 253,17       19  128 /mnt/data
    lsof    2533 root  cwd    DIR 253,17       19  128 /mnt/data
    ```
  * After identifying the processes, wait for the processes to complete or send the SIGTERM or ```SIGKILL``` signal to terminate them. In this case, it is sufficient to change the current working directory to a directory outside the mount point.
    ```console
    [root@host data]# cd
    [root@host ~]# umount /mnt/data
    ```


<a name="15.5"></a>
## 15.5 Locate Files on the System

| Content |
| --- |

<a name=""></a>
* 


