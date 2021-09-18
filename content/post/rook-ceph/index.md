---
title: rook-ceph 踩坑
summary: 
date: "2021-08-17"
authors:
- admin
tags:
- cloud
- kubernetes
categories:
- 总结
---

## k8s 安装rook-ceph

<https://rook.io/docs/rook/v1.7/>

## 添加新磁盘

```bash
#扫描 SCSI总线并添加 SCSI 设备
for host in $(ls /sys/class/scsi_host) ; do echo "- - -" > /sys/class/scsi_host/$host/scan; done

#重新扫描 SCSI 总线
for scsi_device in $(ls /sys/class/scsi_device/); do echo 1 > /sys/class/scsi_device/$scsi_device/device/rescan; done

#查看已添加的磁盘，能够看到sdb说明添加成功
lsblk
```

## 清理集群

关闭pods，才能pvc才能被删除

<https://rook.io/docs/rook/v1.7/ceph-teardown.html>

## 节点磁盘格式化

最好每个磁盘都格式化一下

``` bash
#!/usr/bin/env bash
DISK="/dev/sdd"

# Zap the disk to a fresh, usable state (zap-all is important, b/c MBR has to be clean)

# You will have to run this step for all disks.
sgdisk --zap-all $DISK

# Clean hdds with dd
dd if=/dev/zero of="$DISK" bs=1M count=100 oflag=direct,dsync

# Clean disks such as ssd with blkdiscard instead of dd
blkdiscard $DISK

# These steps only have to be run once on each node
# If rook sets up osds using ceph-volume, teardown leaves some devices mapped that lock the disks.
ls /dev/mapper/ceph-* | xargs -I% -- dmsetup remove %

# ceph-volume setup can leave ceph-<UUID> directories in /dev and /dev/mapper (unnecessary clutter)
rm -rf /dev/ceph-*
rm -rf /dev/mapper/ceph--*

# Inform the OS of partition table changes
partprobe $DISK

```

## 删除

删除Ceph集群后，在之前部署Ceph组件节点的/var/lib/rook/目录，会遗留下Ceph集群的配置信息。
若之后再部署新的Ceph集群，先把之前Ceph集群的这些信息删除，不然启动monitor会失败；

### 坑点

1. 删除前必须先删除所有申请的资源
2. 记得删除申请的文件系统！！！

查看资源
> kubectl api-resources --namespaced=true -o name | xargs -n 1 kubectl get --show-kind --ignore-not-found -n rook-ceph

使用kubectl edit crd clusters.ceph.rook.io编辑
Terminating，把finalizers的值删掉，保存

<https://www.codeleading.com/article/5578440677/>
