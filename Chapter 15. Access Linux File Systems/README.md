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
  * Partitions are block devices in their own right. For example, on the first SATA-attached storage, the first partition is the /dev/sda1 disk. The second partition of the same storage is the /dev/sda2 disk. The third partition on the third SATA-attached storage device is the /dev/sdc3 disk, and so on. Paravirtualized storage devices have a similar naming system. For example, the first partition on the first storage device is the /dev/vda1 disk. The second partition of the second storage device is the /dev/vdb2 disk, and so on.

An NVMe-attached SSD device names its partitions differently from a SATA-attached device. For NVMe storage devices, the nvmeX part of the name refers to the device, the nY part refers to the namespace, and the pZ part refers to the partition. For example, the first partition for the first namespace on the first disk is the /dev/nvme0n1p1 partition. The third partition for the first namespace on the second disk is the /dev/nvme1n1p3 partition, and so on.

SD or MMC cards can sometimes have a similar naming system to the SATA devices (/dev/sdN), but it is not always the case. In some cases, SD or MMC cards might have names such as /dev/mmcblk0p1, where the mmcblkX part of the name refers to the storage device and the pY part of the name refers to the partition number on that device.

An extended listing of the /dev/sda1 device file on the host machine reveals the b file type, which stands for a block device:

[user@host ~]$ ls -l /dev/sda1
brw-rw----. 1 root disk 8, 1 Feb 22 08:00 /dev/sda1
Logical Volumes
Another way of organizing disks and partitions is with Logical Volume Management (LVM). With LVM, it is possible to aggregate block devices into a volume group. Disk space in the volume group is separated into logical volumes, which are the functional equivalent of a partition on a physical disk.

The LVM system assigns names to volume groups and logical volumes on their creation. LVM creates a directory in the /dev directory that matches the group name, and creates a symbolic link within that new directory with the same name as the logical volume. That logical volume file is then available to be mounted. For example, when a myvg volume group and the mylv logical volume are present, the full path to the logical volume is the /dev/myvg/mylv file.

Note
The previously mentioned logical volume device name establishes a symbolic link to the device file that accesses it, which might vary between boots. Another form of logical volume device name, which is linked from files in the /dev/mapper directory, is often used for symbolic links to the device file.


<a name="15.3"></a>
## 15.3 Mount and Unmount File Systems

| Content |
| --- |

<a name=""></a>
* 


<a name="15.5"></a>
## 15.5 Locate Files on the System

| Content |
| --- |

<a name=""></a>
* 


