---
title: Linux Filesystem Structure
description: A beginner's guide to the Ubuntu/Linux root directory structure — what each folder contains and why it exists.
category: linux-filesystem
order: 5
---

## Overview

Linux organizes everything into a single directory tree starting from the root `/`. Unlike Windows, which uses drive letters (`C:\`, `D:\`), every file and device on a Linux system lives somewhere under `/`. Understanding this structure is one of the first steps to becoming comfortable with Ubuntu.

> **Key principle:** In Linux, *everything is a file* — regular files, directories, hardware devices, running processes, and even network sockets are all represented as files somewhere in the filesystem.

## The Root Directory Tree

Here is the complete top-level directory structure you'll find on an Ubuntu system:

```
/
├── bin       → Essential user command binaries
├── boot      → Boot loader files and Linux kernel
├── dev       → Device files (hardware interfaces)
├── etc       → System-wide configuration files
├── home      → User home directories
├── lib       → Essential shared libraries
├── lib64     → 64-bit shared libraries
├── lost+found → Recovered files after filesystem repair
├── media     → Mount point for removable media
├── mnt       → Temporary mount point for filesystems
├── opt       → Optional/third-party software
├── proc      → Virtual filesystem for process info
├── root      → Home directory for the root user
├── run       → Runtime variable data
├── sbin      → Essential system administration binaries
├── snap      → Snap package mount points (Ubuntu-specific)
├── srv       → Data for services (web, FTP, etc.)
├── sys       → Virtual filesystem for kernel/hardware info
├── tmp       → Temporary files (cleared on reboot)
├── usr       → User programs, libraries, and documentation
└── var       → Variable data (logs, caches, mail, databases)
```

## Directory-by-Directory Breakdown

### `/bin` — Essential User Binaries

Contains fundamental commands that all users need, even in single-user mode or during system recovery. These are the commands you use every day.

**Examples:**
```bash
ls        # List directory contents
cp        # Copy files
mv        # Move/rename files
rm        # Remove files
cat       # Display file contents
grep      # Search text patterns
chmod     # Change file permissions
echo      # Print text
bash      # The Bash shell itself
```

> **Note:** On modern Ubuntu, `/bin` is a symbolic link to `/usr/bin`. They contain the same files.

---

### `/boot` — Boot Loader Files

Contains everything needed to start the system before the kernel hands off to the init process. You generally should not modify files here unless you know what you're doing.

**Contains:**
- `vmlinuz` — The compressed Linux kernel
- `initrd.img` — Initial RAM disk (loaded before root filesystem)
- `grub/` — GRUB bootloader configuration and modules
- `config-*` — Kernel configuration files
- `System.map` — Kernel symbol table

```bash
# List kernel versions installed
ls /boot/vmlinuz-*

# View GRUB configuration
cat /boot/grub/grub.cfg
```

---

### `/dev` — Device Files

Virtual directory where the kernel exposes hardware devices as files. Every piece of hardware — disks, USB drives, keyboards, GPUs — gets a file here.

**Common devices:**
| Device | Description |
|--------|-------------|
| `/dev/sda` | First SATA/SCSI disk |
| `/dev/sda1` | First partition on first disk |
| `/dev/nvme0n1` | First NVMe SSD |
| `/dev/null` | Discards all data written to it (the "black hole") |
| `/dev/zero` | Produces infinite zero bytes |
| `/dev/random` | Cryptographically secure random data |
| `/dev/urandom` | Fast pseudo-random data |
| `/dev/tty` | Current terminal |
| `/dev/loop0` | First loop device (for mounting images) |

```bash
# List all block devices (disks and partitions)
lsblk

# Send output to the void
echo "discarded" > /dev/null
```

---

### `/etc` — System Configuration

The most important directory for system configuration. Nearly every system-wide setting lives here. The name historically stands for "et cetera" but is often interpreted as "Editable Text Configuration."

**Key configuration files:**
| File/Directory | Purpose |
|---------------|---------|
| `/etc/hostname` | Your computer's hostname |
| `/etc/hosts` | Local DNS mappings |
| `/etc/fstab` | Disk mount configuration (loaded at boot) |
| `/etc/passwd` | User account information |
| `/etc/shadow` | Encrypted user passwords |
| `/etc/group` | Group definitions |
| `/etc/sudoers` | Sudo permission rules |
| `/etc/apt/sources.list` | APT package repository list |
| `/etc/ssh/sshd_config` | SSH server configuration |
| `/etc/environment` | System-wide environment variables |
| `/etc/crontab` | System-wide scheduled tasks |
| `/etc/default/grub` | GRUB bootloader defaults |
| `/etc/NetworkManager/` | Network configuration |
| `/etc/systemd/` | Systemd service configuration |

```bash
# View your hostname
cat /etc/hostname

# View filesystem mount table
cat /etc/fstab

# List all user accounts
cat /etc/passwd
```

> **Warning:** Always back up files in `/etc` before editing them. A misconfigured `/etc/fstab` can prevent your system from booting.

---

### `/home` — User Home Directories

Each user gets their own directory under `/home`. This is where all your personal files, configurations, and data live.

```
/home/
└── yourusername/
    ├── Desktop/
    ├── Documents/
    ├── Downloads/
    ├── Music/
    ├── Pictures/
    ├── Videos/
    ├── .bashrc          # Shell configuration
    ├── .profile         # Login environment
    ├── .config/         # App configurations (XDG standard)
    ├── .local/          # User-installed data and programs
    ├── .ssh/            # SSH keys and config
    └── .cache/          # Application caches
```

The `~` (tilde) character is a shortcut for your home directory:
```bash
cd ~           # Go to your home directory
cd ~/Documents # Go to your Documents folder
ls ~/.config   # List your app configurations
```

> **Tip:** Files and directories starting with `.` are hidden. Use `ls -a` to see them.

---

### `/lib` and `/lib64` — Shared Libraries

Contains shared library files (similar to `.dll` files on Windows) that programs in `/bin` and `/sbin` need to run. `/lib64` contains 64-bit versions.

```bash
# See what libraries a program needs
ldd /usr/bin/ls
```

> **Note:** On modern Ubuntu, `/lib` is a symbolic link to `/usr/lib`.

---

### `/lost+found` — Recovered Files

Created automatically on each partition formatted with ext4. After a system crash or filesystem corruption, the `fsck` (filesystem check) tool places recovered file fragments here. You usually won't find anything in it.

---

### `/media` — Removable Media

Where Ubuntu automatically mounts removable devices like USB drives, external hard drives, and optical discs.

```bash
# After plugging in a USB named "MyDrive":
ls /media/yourusername/MyDrive/

# Safely unmount a USB drive
umount /media/yourusername/MyDrive
```

---

### `/mnt` — Temporary Mounts

A conventional mount point for temporarily mounting filesystems manually. Unlike `/media` (which is automatic), `/mnt` is used when you manually mount something.

```bash
# Mount a disk partition manually
sudo mount /dev/sdb1 /mnt

# Mount a network share
sudo mount -t nfs server:/share /mnt

# Unmount when done
sudo umount /mnt
```

---

### `/opt` — Optional Software

Used for installing third-party or add-on software packages that don't come from Ubuntu's package manager. Each application gets its own subdirectory.

**Common examples:**
```
/opt/
├── google/chrome/      # Google Chrome (when installed via .deb)
├── discord/            # Discord
├── sublime_text/       # Sublime Text
└── containerd/         # Container runtime
```

---

### `/proc` — Process Information (Virtual)

A **virtual filesystem** — it doesn't exist on disk. The kernel generates its contents on-the-fly to expose information about running processes and the system.

**Useful files:**
| Path | Shows |
|------|-------|
| `/proc/cpuinfo` | CPU details (model, cores, speed) |
| `/proc/meminfo` | Memory usage statistics |
| `/proc/version` | Kernel version string |
| `/proc/uptime` | System uptime in seconds |
| `/proc/loadavg` | System load averages |
| `/proc/[PID]/` | Information about a specific process |

```bash
# View CPU information
cat /proc/cpuinfo

# Check total and available memory
cat /proc/meminfo | head -5

# See how long the system has been running
cat /proc/uptime

# View info about process 1 (systemd)
ls /proc/1/
```

---

### `/root` — Root User's Home

The home directory for the `root` (superuser) account. It's separate from `/home` so the root user can still log in even if the `/home` partition fails to mount.

```bash
# Access (requires root privileges)
sudo ls /root
```

> **Note:** On Ubuntu, the root account is disabled by default. Use `sudo` instead of logging in as root.

---

### `/run` — Runtime Data

A tmpfs (RAM-based) filesystem that stores volatile runtime information since the last boot. It is cleared on every reboot.

**Contains:**
- PID files for running services
- Socket files for inter-process communication
- Lock files
- Temporary mount points

---

### `/sbin` — System Binaries

Contains essential commands used for system administration — typically only run by the root user or with `sudo`.

**Examples:**
```bash
fdisk     # Disk partitioning
mkfs      # Create filesystems
iptables  # Firewall rules
reboot    # Restart the system
shutdown  # Power off the system
mount     # Mount filesystems
systemctl # Manage systemd services
```

> **Note:** On modern Ubuntu, `/sbin` is a symbolic link to `/usr/sbin`.

---

### `/snap` — Snap Packages (Ubuntu-Specific)

Contains mount points for [Snap](https://snapcraft.io/) packages — Ubuntu's universal packaging format. Each installed snap gets a read-only squashfs mount here.

```bash
# List installed snaps
snap list

# Snap data is stored separately:
# /snap/<name>/current/  → Application files (read-only)
# ~/snap/<name>/         → User data for the snap
```

---

### `/srv` — Service Data

Stores data served by system services. For example, if you're running a web server, you might put website files here.

```
/srv/
├── http/     # Web server files
├── ftp/      # FTP server files
└── git/      # Git repositories (for git server)
```

> **Note:** Many administrators use `/var/www` for web files instead, which has become the more common convention.

---

### `/sys` — System Information (Virtual)

Another virtual filesystem (like `/proc`) that provides information about the kernel, hardware devices, and drivers in a structured way.

```bash
# View CPU temperature (if supported)
cat /sys/class/thermal/thermal_zone0/temp

# View screen brightness
cat /sys/class/backlight/*/brightness

# View block device info
ls /sys/block/
```

---

### `/tmp` — Temporary Files

A directory for temporary files. Any user or program can write here. Files are typically deleted on reboot (Ubuntu uses a tmpfs for this by default).

```bash
# Create a temporary file
echo "test" > /tmp/myfile.txt

# Programs use it for scratch space
# Example: compilers store intermediate files here
```

> **Security note:** Because `/tmp` is world-writable, be cautious about sensitive data here. Use `mktemp` for safe temporary files.

---

### `/usr` — User Programs

The largest directory on most systems. Contains the majority of user-facing programs, libraries, documentation, and shared data. Think of it as a secondary hierarchy mirroring the root structure.

```
/usr/
├── bin/       # Most user commands (gcc, python, git, etc.)
├── sbin/      # Non-essential system commands
├── lib/       # Libraries for /usr/bin and /usr/sbin
├── include/   # C/C++ header files
├── local/     # Locally compiled software (not from apt)
│   ├── bin/
│   ├── lib/
│   └── share/
├── share/     # Architecture-independent data
│   ├── applications/  # .desktop files
│   ├── doc/           # Package documentation
│   ├── man/           # Manual pages
│   ├── icons/         # System icon themes
│   └── fonts/         # System fonts
└── src/       # Source code (e.g., kernel headers)
```

The distinction between `/usr/bin` and `/usr/local/bin`:
- `/usr/bin` — Programs installed by `apt` (the package manager)
- `/usr/local/bin` — Programs you compile and install manually

---

### `/var` — Variable Data

Contains files that are expected to grow and change during system operation.

```
/var/
├── log/       # System and application logs
│   ├── syslog       # Main system log
│   ├── auth.log     # Authentication logs
│   ├── kern.log     # Kernel logs
│   ├── apt/         # APT package manager logs
│   └── journal/     # Systemd journal logs
├── cache/     # Application caches
│   └── apt/         # Downloaded .deb packages
├── lib/       # State information for programs
│   ├── dpkg/        # Package manager database
│   └── apt/         # APT lists and state
├── mail/      # User mailboxes
├── spool/     # Queued data (print jobs, cron, etc.)
├── tmp/       # Temporary files preserved between reboots
└── www/       # Web server document root (Apache/Nginx)
```

```bash
# View recent system logs
sudo tail -f /var/log/syslog

# Check login attempts
sudo cat /var/log/auth.log | tail -20

# See how much space cached packages use
du -sh /var/cache/apt/archives/

# Clean up cached packages
sudo apt clean
```

---

## Quick Reference Table

| Directory | Purpose | Typical User Access |
|-----------|---------|-------------------|
| `/bin` | Essential commands | Read + Execute |
| `/boot` | Kernel and bootloader | Read only |
| `/dev` | Device files | Through programs |
| `/etc` | System configuration | Read (sudo to edit) |
| `/home` | Your personal files | Full access |
| `/lib` | Shared libraries | Read only |
| `/media` | USB drives, discs | Full access |
| `/mnt` | Manual mounts | sudo required |
| `/opt` | Third-party apps | Read + Execute |
| `/proc` | Process/system info | Read only |
| `/root` | Root user's home | sudo required |
| `/run` | Runtime data | Read only |
| `/sbin` | Admin commands | sudo required |
| `/snap` | Snap packages | Read only |
| `/srv` | Service data | sudo required |
| `/sys` | Hardware/kernel info | Read only |
| `/tmp` | Temporary files | Full access |
| `/usr` | User programs | Read + Execute |
| `/var` | Logs, caches, data | Varies |

## Navigating the Filesystem

Essential commands for moving around:

```bash
pwd                 # Print current directory
cd /path/to/dir     # Change directory
cd ~                # Go to home directory
cd ..               # Go up one level
cd -                # Go to previous directory

ls                  # List files
ls -la              # List all files with details
ls -lh              # Human-readable file sizes

tree                # Visual directory tree (install: sudo apt install tree)
tree -L 2           # Tree with depth limit of 2

find / -name "*.conf" -type f   # Find all .conf files
locate filename                  # Fast file lookup (install: sudo apt install mlocate)

df -h               # Disk usage by filesystem
du -sh /path        # Size of a directory
```

## Understanding File Permissions

Every file in Linux has permissions shown as a 10-character string:

```
-rwxr-xr-x  1  owner  group  size  date  filename
│├──┤├──┤├──┤
│  │   │   └── Others: read + execute
│  │   └────── Group: read + execute
│  └────────── Owner: read + write + execute
└───────────── Type: - file, d directory, l symlink
```

| Symbol | Permission | Numeric |
|--------|-----------|---------|
| `r` | Read | 4 |
| `w` | Write | 2 |
| `x` | Execute | 1 |
| `-` | None | 0 |

```bash
# Common permission patterns
chmod 755 file    # rwxr-xr-x (owner full, others read+execute)
chmod 644 file    # rw-r--r-- (owner read+write, others read only)
chmod 700 dir     # rwx------ (owner only, private)
chmod +x script   # Add execute permission for everyone
```

> **Tip:** Use `stat filename` to see detailed permission and ownership information for any file.
