---
title: Volumes & Drive Management
description: Learn how to list, mount, format, partition, and monitor drives and volumes in Linux/Ubuntu.
category: volumes-drives
order: 6
---

## Overview

Linux treats everything as a file, including hard drives and storage devices. Drives appear as block devices under `/dev/` and must be mounted to a directory before you can access their contents. This guide covers the essential tools and concepts for managing storage in Ubuntu.

## Key Concepts

### Block Devices

Physical and virtual drives appear in `/dev/` with names like:

| Device | Description |
|--------|-------------|
| `/dev/sda` | First SATA/SCSI/USB drive |
| `/dev/sdb` | Second SATA/SCSI/USB drive |
| `/dev/nvme0n1` | First NVMe SSD |
| `/dev/nvme0n1p1` | First partition on first NVMe SSD |
| `/dev/sda1` | First partition on first SATA drive |
| `/dev/loop0` | Loop device (snap packages, ISOs) |

### Mount Points

A mount point is a directory where a filesystem is attached. Common mount points:

| Mount Point | Purpose |
|-------------|---------|
| `/` | Root filesystem |
| `/home` | User home directories |
| `/boot` | Boot loader files |
| `/boot/efi` | EFI system partition |
| `/mnt` | Temporary manual mounts |
| `/media/$USER` | Auto-mounted removable media |

## Checking Disk Space

### df — Disk Free

The most common way to check available disk space:

```bash
# Human-readable sizes for all mounted filesystems
df -h

# Show only specific filesystem types (exclude snaps and tmpfs)
df -h -x squashfs -x tmpfs -x devtmpfs

# Check space on a specific path
df -h /home

# Show filesystem types
df -hT
```

### du — Disk Usage

Check how much space files and directories are using:

```bash
# Size of current directory
du -sh .

# Size of each item in current directory, sorted
du -sh * | sort -h

# Find the 10 largest directories under /home
du -h /home --max-depth=2 | sort -rh | head -10

# Size of a specific directory
du -sh /var/log
```

### ncdu — Interactive Disk Usage

A TUI tool for exploring disk usage interactively:

```bash
# Install ncdu
sudo apt install ncdu

# Scan and browse current directory
ncdu

# Scan a specific path
ncdu /home

# Scan root filesystem (excluding virtual filesystems)
sudo ncdu / --exclude /proc --exclude /sys --exclude /dev
```

## Listing Drives & Partitions

### lsblk — List Block Devices

```bash
# Basic tree view of all block devices
lsblk

# Show filesystem type, label, and UUID
lsblk -f

# Show device sizes and mount points
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT,FSTYPE

# Show all columns
lsblk -a
```

### blkid — Block Device IDs

```bash
# List all block devices with UUIDs and filesystem types
sudo blkid

# Get info for a specific device
sudo blkid /dev/sda1

# Output in a clean format
sudo blkid -o list
```

### fdisk — Partition Table Info

```bash
# List all disks and partitions
sudo fdisk -l

# List partitions on a specific disk
sudo fdisk -l /dev/sda
```

## Mounting & Unmounting

### Manual Mounting

```bash
# Mount a partition to a directory
sudo mount /dev/sdb1 /mnt

# Mount with a specific filesystem type
sudo mount -t ntfs-3g /dev/sdb1 /mnt

# Mount read-only
sudo mount -ro /dev/sdb1 /mnt

# Mount an ISO file
sudo mount -o loop image.iso /mnt

# Unmount
sudo umount /mnt

# Force unmount (if busy)
sudo umount -l /mnt
```

### Automatic Mounting with /etc/fstab

The `/etc/fstab` file controls which filesystems are mounted at boot:

```bash
# View current fstab
cat /etc/fstab

# Edit fstab (always make a backup first)
sudo cp /etc/fstab /etc/fstab.backup
sudo nano /etc/fstab
```

An fstab entry has this format:

```
<device>       <mount-point>  <fs-type>  <options>       <dump>  <pass>
UUID=abc-123   /data          ext4       defaults        0       2
/dev/sdb1      /backup        ext4       defaults,noauto 0       0
```

> Use `UUID=` instead of `/dev/sdX` in fstab — device names can change between boots, but UUIDs are stable. Get the UUID with `sudo blkid`.

```bash
# Test fstab without rebooting (mount all entries)
sudo mount -a

# Mount a specific fstab entry
sudo mount /data
```

### Common fstab Options

| Option | Description |
|--------|-------------|
| `defaults` | rw, suid, dev, exec, auto, nouser, async |
| `noauto` | Don't mount at boot (manual mount only) |
| `nofail` | Don't fail boot if device is missing |
| `ro` | Mount read-only |
| `rw` | Mount read-write |
| `user` | Allow non-root users to mount |
| `uid=1000,gid=1000` | Set owner (useful for NTFS/FAT) |

## Partitioning Drives

> **Warning**: Partitioning can destroy data. Always back up before modifying partitions.

### fdisk — Interactive Partitioning (MBR/GPT)

```bash
# Start fdisk on a drive
sudo fdisk /dev/sdb

# Common fdisk commands (inside the interactive prompt):
# p — print partition table
# n — create new partition
# d — delete a partition
# t — change partition type
# w — write changes and exit
# q — quit without saving
```

### parted — Partition Editor (GPT)

```bash
# Start parted on a drive
sudo parted /dev/sdb

# Print partition table
sudo parted /dev/sdb print

# Create a GPT partition table
sudo parted /dev/sdb mklabel gpt

# Create a partition using the full disk
sudo parted /dev/sdb mkpart primary ext4 0% 100%
```

### GParted — Graphical Partition Editor

```bash
# Install GParted
sudo apt install gparted

# Launch GParted
sudo gparted
```

## Formatting Drives

### Creating Filesystems

```bash
# Format as ext4 (standard Linux filesystem)
sudo mkfs.ext4 /dev/sdb1

# Format as ext4 with a label
sudo mkfs.ext4 -L "MyData" /dev/sdb1

# Format as NTFS (for Windows compatibility)
sudo mkfs.ntfs /dev/sdb1

# Format as FAT32 (USB drives, EFI partitions)
sudo mkfs.vfat -F 32 /dev/sdb1

# Format as exFAT (cross-platform, large files)
sudo mkfs.exfat /dev/sdb1

# Format as XFS
sudo mkfs.xfs /dev/sdb1

# Format as Btrfs
sudo mkfs.btrfs /dev/sdb1
```

### Filesystem Comparison

| Filesystem | Max File Size | Use Case |
|-----------|---------------|----------|
| ext4 | 16 TB | Standard Linux — reliable, well-supported |
| Btrfs | 16 EB | Snapshots, compression, modern Linux |
| XFS | 8 EB | Large files, high performance |
| NTFS | 16 TB | Windows drives, dual-boot |
| FAT32 | 4 GB | USB drives, EFI, max compatibility |
| exFAT | 16 EB | Large USB drives, cross-platform |

## Monitoring Drive Health

### SMART Monitoring

```bash
# Install smartmontools
sudo apt install smartmontools

# Check SMART health status
sudo smartctl -H /dev/sda

# View all SMART data
sudo smartctl -a /dev/sda

# Run a short self-test
sudo smartctl -t short /dev/sda

# Run a long self-test
sudo smartctl -t long /dev/sda

# View test results
sudo smartctl -l selftest /dev/sda
```

### Monitoring I/O Activity

```bash
# Real-time disk I/O stats (install with: sudo apt install sysstat)
iostat -x 1

# Interactive I/O monitor
sudo iotop

# Watch disk activity
watch -n 1 "cat /proc/diskstats"
```

## Working with Swap

Swap space is used when physical RAM is full:

```bash
# Check current swap
swapon --show
free -h

# Create a swap file (2GB example)
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Make permanent — add to /etc/fstab:
# /swapfile none swap sw 0 0

# Disable swap
sudo swapoff /swapfile

# Adjust swappiness (how aggressively Linux uses swap, 0-100)
cat /proc/sys/vm/swappiness          # check current value
sudo sysctl vm.swappiness=10         # set temporarily
# For permanent: add vm.swappiness=10 to /etc/sysctl.conf
```

## USB & Removable Drives

USB drives are typically auto-mounted under `/media/$USER/` in Ubuntu:

```bash
# List USB devices
lsusb

# Watch for new devices
dmesg --follow

# Safely eject a USB drive
udisksctl unmount -b /dev/sdb1
udisksctl power-off -b /dev/sdb

# Or using the command line
sudo eject /dev/sdb
```

## LVM (Logical Volume Manager)

LVM allows flexible disk management with resizable volumes:

```bash
# Check if LVM tools are installed
sudo apt install lvm2

# View physical volumes, volume groups, and logical volumes
sudo pvs        # physical volumes
sudo vgs        # volume groups
sudo lvs        # logical volumes

# Detailed info
sudo pvdisplay
sudo vgdisplay
sudo lvdisplay

# Extend a logical volume (add 10GB)
sudo lvextend -L +10G /dev/vgname/lvname
sudo resize2fs /dev/vgname/lvname    # resize ext4 filesystem
```

## Useful Disk Commands Reference

| Command | Description |
|---------|-------------|
| `lsblk` | List block devices in tree format |
| `df -h` | Show disk space usage |
| `du -sh *` | Show sizes of items in current directory |
| `sudo fdisk -l` | List all partitions |
| `sudo blkid` | Show UUIDs and filesystem types |
| `mount` | Show all mounted filesystems |
| `findmnt` | Show mount tree |
| `findmnt -t ext4` | Show only ext4 mounts |
| `cat /proc/partitions` | Show kernel's partition table |
| `sudo hdparm -I /dev/sda` | Show drive hardware info |
| `sudo tune2fs -l /dev/sda1` | Show ext filesystem details |

## Best Practices

- **Always use UUIDs** in `/etc/fstab` instead of device names like `/dev/sda1`
- **Back up before partitioning** — partition operations can destroy data
- **Use `nofail`** in fstab for non-essential drives so boot doesn't fail if a drive is missing
- **Monitor SMART data** regularly for early warning of drive failure
- **Use `ncdu`** for finding what's eating your disk space — much faster than manual `du` commands
- **Unmount before formatting** — never format a mounted partition
- **Test fstab changes** with `sudo mount -a` before rebooting

> See the [Volumes & Drives Cheat Sheet](/cheatsheets/volumes-drives) for a quick command reference.
