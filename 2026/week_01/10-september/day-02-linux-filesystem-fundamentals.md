# Day 02 — Linux Filesystem Fundamentals

**Date:** 2026-09-10

---

## 🎯 Objective

**What was I trying to accomplish today?**

* Understand the basic Linux filesystem hierarchy.
* Learn how to create and inspect files from the terminal.
* Understand the purpose of important system directories.
* Learn how Linux represents disks, partitions, and filesystems.
* Become familiar with tools used for inspecting and partitioning disks.

---

## 📚 What I Learned

### 1. Reading Terminal Input into a File

The `cat` command can be used to create a file directly from terminal input.

#### Create or overwrite a file

```bash
cat > newfile.txt
```

Enter your text and press **`Ctrl+D`** to finish.

If `newfile.txt` already exists, the `>` operator **overwrites its contents**.

#### Append to an existing file

To add content to the end of a file without overwriting it:

```bash
cat >> newfile.txt
```

---

### 2. Formatting the Output

The `cat` command provides several options that make file contents easier to inspect.

| Option | Description                                               |
| ------ | --------------------------------------------------------- |
| `-n`   | Numbers every line, including blank lines                 |
| `-b`   | Numbers only non-empty lines                              |
| `-s`   | Combines consecutive blank lines into a single blank line |
| `-A`   | Displays hidden characters, such as tabs and line endings |

Example:

```bash
cat -n file.txt
```

> **Note:** These options only affect how the output is displayed. They **do not modify the original file**.

---

## 🗂️ Linux Filesystem Hierarchy

Linux organizes files and directories in a hierarchical structure that starts at the root directory.

### Root and Essential System Paths

| Path    | Purpose                                                                                               |
| ------- | ----------------------------------------------------------------------------------------------------- |
| `/`     | The top-level directory of the filesystem hierarchy                                                   |
| `/etc`  | Contains system-wide configuration files and may also contain executable scripts or helpers           |
| `/boot` | Stores files required during system startup, including bootloader data, kernels, and initramfs images |
| `/bin`  | Traditionally contains essential user commands                                                        |
| `/sbin` | Traditionally contains essential system-administration utilities                                      |
| `/lib`  | Traditionally contains essential shared libraries and system loader components                        |

> **Modern Linux distributions:** `/bin`, `/sbin`, and `/lib` may be symbolic links into `/usr`.

### User and Service Directories

| Path    | Purpose                                                  |
| ------- | -------------------------------------------------------- |
| `/home` | Conventionally contains non-root users' home directories |
| `/root` | Conventional home directory of the `root` account        |
| `/srv`  | Intended for site-specific data served by the system     |

> The location of user home directories can vary depending on directory services and local system policy.

---

## 📦 Variable, Runtime, and Temporary Data

### `/var`

Contains **variable data**, such as:

* Logs
* Caches
* Spools
* Application state

System logs commonly appear under:

```text
/var/log
```

However, some systems rely primarily on a journal interface such as `systemd-journald`.

### `/run`

Contains **volatile runtime state** for the current boot, such as:

* Unix sockets
* Service state
* PID files

It is normally recreated during boot.

### `/tmp`

Used for temporary files.

It is commonly writable by all users and typically uses **sticky-bit protection**.

### `/var/tmp`

Intended for temporary files that should generally survive longer than files stored in `/tmp`.

> **Important:** Cleanup policies for `/tmp` vary between systems. Do not assume that files always persist until reboot or are always deleted at reboot.
>
> Applications should use secure temporary-file creation mechanisms rather than predictable filenames.

---

## 🔌 Devices, Kernel Interfaces, and Mount Points

| Path     | Purpose                                                                  |
| -------- | ------------------------------------------------------------------------ |
| `/dev`   | Contains device nodes and related runtime links                          |
| `/proc`  | Exposes process and kernel interfaces through `procfs`                   |
| `/sys`   | Exposes kernel objects, devices, drivers, and attributes through `sysfs` |
| `/media` | Commonly used for automatically mounted removable media                  |
| `/mnt`   | Conventional location for temporary administrator mounts                 |

---

## 🔍 Discovering Active Filesystem Types

To display currently mounted filesystems and their types:

```bash
findmnt -o TARGET,SOURCE,FSTYPE,OPTIONS
```

### Other useful commands

#### `df -T`

Displays mounted filesystem information together with filesystem types and space usage:

```bash
df -T
```

#### `lsblk -f`

Displays block devices together with filesystem information:

```bash
lsblk -f
```

#### `/proc/filesystems`

Shows filesystem types supported or known by the running kernel:

```bash
cat /proc/filesystems
```

### Important distinction

These commands answer **different questions**:

* `findmnt` → What filesystems are currently mounted?
* `df -T` → How much space is available on mounted filesystems?
* `lsblk -f` → What block devices and filesystem signatures are present?
* `/proc/filesystems` → What filesystem types does the running kernel support/know about?

> An **unmounted filesystem** will not appear in an ordinary mounted-filesystem listing.

---

# 💽 Partition Tables and Disk Boundaries

A **partition table** describes how a disk is divided into partitions.

It records information such as:

* Start positions
* Partition sizes/lengths
* Partition type identifiers
* Scheme-specific attributes

The Linux kernel reads the partition table and creates corresponding **partition block devices**.

For example:

```text
/dev/sda1
/dev/sda2
/dev/nvme0n1p1
/dev/nvme0n1p2
```

---

## ⚠️ Anatomy of a Disk — Must Be Reviewed

> **This topic needs further review.**

Before working with partitioning tools, I need to understand the relationship between:

```text
Physical Disk
      ↓
Partition Table
      ↓
Partitions
      ↓
Filesystems
      ↓
Mount Points
      ↓
Files & Directories
```

A partition is **not the same thing as a filesystem**.

For example:

```text
/dev/sda1
    ↓
ext4 filesystem
    ↓
mounted at /home
```

---

# 🧩 MBR Partitioning

> **Needs further explanation and review.**

**MBR** stands for **Master Boot Record** and refers to the traditional DOS/MBR partitioning scheme.

The legacy MBR partition table is stored in the first sector of the disk.

It provides **four primary partition entries**.

### Extended partitions

One of those entries can instead describe an **extended partition**.

The extended partition acts as a container for a linked series of **logical partitions**, allowing systems using the MBR scheme to create more than four usable partitions.

Conceptually:

```text
Disk
│
├── Primary Partition
├── Primary Partition
├── Primary Partition
└── Extended Partition
     ├── Logical Partition
     ├── Logical Partition
     └── Logical Partition
```

### Things to review

* MBR structure
* Boot code
* Partition table entries
* Primary partitions
* Extended partitions
* Logical partitions
* MBR limitations

---

# 🧩 GPT Partitioning

> **Needs further explanation and review.**

**GPT** stands for **GUID Partition Table** and is the modern partitioning scheme commonly used with UEFI-based systems.

### Things to review

* GPT structure
* Protective MBR
* Primary GPT header
* Partition entry array
* Backup GPT header
* GUIDs
* Partition attributes
* Maximum partition sizes
* Maximum number of partitions
* Relationship between GPT and UEFI

Conceptually:

```text
Disk
│
├── Protective MBR
├── GPT Header
├── Partition Entry Array
├── Partition Data
│
├── ...
│
├── Backup Partition Entry Array
└── Backup GPT Header
```

---

# 🛠️ Disk Partitioning Tools

Several tools can be used to inspect or modify partition tables.

| Tool      | Description                                                              |
| --------- | ------------------------------------------------------------------------ |
| `fdisk`   | Command-line partitioning utility supporting MBR and GPT                 |
| `parted`  | Partitioning utility supporting GPT, MBR, and other partitioning schemes |
| `gdisk`   | Command-line tool focused primarily on GPT                               |
| `GParted` | Graphical frontend for partitioning and filesystem operations            |

### Quick comparison

```text
fdisk
  └── CLI
  └── MBR + GPT

parted
  └── CLI
  └── MBR + GPT + other schemes

gdisk
  └── CLI
  └── GPT-focused

GParted
  └── GUI
  └── Partition + filesystem management
```

> ⚠️ **Warning:** Partitioning tools can modify the structure of a disk and may cause data loss if used incorrectly. Always verify the target disk before applying changes.

---

# 🔨 What I Built

- [x] Created a Linux filesystem practice workspace
- [x] Created nested directories using `mkdir`
- [x] Created multiple test files using `touch`
- [x] Copied files between directories using `cp`
- [x] Moved and renamed files using `mv`
- [x] Safely removed test files using `rm`
- [x] Navigated the filesystem using absolute and relative paths

## 📍 Absolute vs Relative Paths

An absolute path starts from the root directory `/`.

```bash
cd /home/user/projects
---

# 💡 Key Takeaway

Linux follows a hierarchical filesystem structure in which different directories have specific purposes.

I also learned the distinction between:

```text
Disk
  ↓
Partition Table
  ↓
Partition
  ↓
Filesystem
  ↓
Mount Point
  ↓
Files
```

Understanding this relationship is essential before working with disks, filesystems, mounting, and partitioning tools.

---

# 🔗 Resources

# Linux Fundamentals

Resources for learning and practicing Linux fundamentals.

## Linux Filesystem

### Linux Journey
https://labex.io/linuxjourney

Topics:
- Filesystem hierarchy
- Navigation
- Files and directories
- Permissions
- Processes
- Basic shell usage

Why I use it:
Interactive Linux exercises focused on command-line fundamentals.

---

# ➡️ Next

* Review the anatomy of a disk.
* Understand **MBR vs GPT** in more depth.
* Learn how partitions relate to filesystems.
* Practice identifying disks and partitions with:

  ```bash
  lsblk
  lsblk -f
  ```
* Inspect mounted filesystems with:

  ```bash
  findmnt
  df -T
  ```
* Practice reading partition tables with:

  ```bash
  fdisk -l
  ```
* Learn the difference between **partitioning, formatting, and mounting**.
