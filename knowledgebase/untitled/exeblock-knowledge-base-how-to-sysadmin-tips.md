# EXEBlock Knowledge Base : How to - SysAdmin tips -

List of useful links, commands and step-by-step guides

## VM management server <a id="Howto-SysAdmintips--VMmanagementserver"></a>

[https://whmcs.datasecuritynode.com/whmcs/clientarea.php](https://whmcs.datasecuritynode.com/whmcs/clientarea.php)

## Resize disk space on Ubuntu VM <a id="Howto-SysAdmintips--ResizediskspaceonUbuntuVM"></a>

[https://pve.proxmox.com/wiki/Resize\_disks](https://pve.proxmox.com/wiki/Resize_disks)

Every time you expand or reduce space in a linux partition you have to resize it in partition and filesystem.

$ sudo -s  
$ df -h

### Partition manager: <a id="Howto-SysAdmintips--Partitionmanager:"></a>

1.  $ parted /dev/sda

\(parted\) print

\(parted\) resizepart 1 100% 

 _you should make sure that no other partitions preventing from extending /dev/sda1_                

\(parted\) quit     

2.  $ fdisk -l

### Filesystem manager <a id="Howto-SysAdmintips--Filesystemmanager"></a>

$ resize2fs /dev/sda1  
by default it should take 100% of whatever space is available

```text
sudo -s
df -h
parted /dev/sda
(parted) print                                                            
(parted) quit
fdisk -l
fdisk -l /dev/sda | grep ^/dev
resize2fs
resize2fs /dev/sda1
```

