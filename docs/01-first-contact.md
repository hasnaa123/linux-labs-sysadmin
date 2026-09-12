# Milestone 1 — First Contact

## Baseline
| Fact | Command | Value |
|---|---|---|
| Username |whoami | sysadmin |
| Hostname |hostname | ubuntu-lab |
| Ubuntu Version |lsb_release -d | 22.04.5 LTS |
| Kernel Version |uname -r| 5.15.0-187-generic |
| Uptime |uptime -p | 9h 21m (at time of check) |
| Disk (root fs)| df -h / | 8.1G total, 3.4G used, 4.3G available |
| RAM | free -h | 3.8Gi total, 258Mi used, 3.2Gi available |

## Filesystem Exploration
**The Filesystem Hierarchy Standard (FHS)**
Linux has one unified tree rooted at `/` — no drive letters. Other storage gets *mounted into* this tree. Key directories to understand the *purpose* of, not just memorize:

- `/etc` — system-wide configuration
- `/var` — variable data: logs, caches, spool files (things that grow/change)
- `/home` — regular users' personal directories
- `/root` — the root user's home (deliberately separate from `/home`)
- `/bin`, `/sbin`, `/usr/bin`, `/usr/sbin` — executables, historically split by essential-vs-not and user-vs-admin (modern Ubuntu merges some of this — worth discovering why)
- `/tmp` — temporary files, often cleared on reboot
- `/opt` — optional/third-party software
- `/dev` — device files ("everything is a file" made literal)
- `/proc`, `/sys` — virtual filesystems exposing live kernel/process info, not real files on disk

## /proc and /sys
ls -l /home : presents a storage  size

ls -l /proc  : files with 0 bytes storage

ls -l  /sys   :  files with 0 bytes storage

After comparison files on /home present actual storage space because they are real files stored on disk. Instead, the files on /proc and /sys don't present actual storage size because they aren't real files stored on disk they are generated live by the kernel from information it already holds in memory (CPU info, running processes hardware state, etc.), related to the system that changes every second. So it's pointless to save this data to disk since it would need constant rewriting and would be outdated the moment it was saved instead the kernel just generates it on demand when read.

## File Operations
check : [`journal/01-first-contact.md`](journal/01-first-contact.md)

# File & Directory Operations — Command Reference

## Create directories

```bash
mkdir -p practice/projectA/drafts
mkdir -p practice/projectB
mkdir -p practice/archive
```

## Create files

```bash
touch practice/projectA/notes.txt
touch practice/projectB/notes.txt
```

## Edit files

```bash
nano notes.txt
vim notes.txt
```

* `nano` → simple, beginner-friendly editor
* `vim` → modal, powerful terminal editor

## Rename / Move

```bash
mv projectB projectB-old
```

Rename or move a file/directory.

## Copy

```bash
cp file.txt destination/
cp file1.txt file2.txt destination/
cp -r directory/ destination/
cp -i file.txt destination/
```

* `cp` → copy files
* `cp -r` → copy directories recursively
* `cp -i` → ask before overwriting

## Move

```bash
mv file.txt destination/
```

Moves a file or directory. The original location no longer contains it.

## Delete an empty directory

```bash
rmdir directory/
```

`rmdir` only removes **empty directories**.

## Delete a file

```bash
rm file.txt
```

## Delete a directory recursively

```bash
rm -r directory/
```

`rm -r` removes a directory and its contents.

⚠️ **Always verify the path before using `rm -r`.**

## Useful distinction

| Command | Purpose                      |
| ------- | ---------------------------- |
| `mkdir` | Create directory             |
| `touch` | Create empty file            |
| `cp`    | Copy                         |
| `mv`    | Move / rename                |
| `rmdir` | Delete empty directory       |
| `rm`    | Delete file                  |
| `rm -r` | Delete directory recursively |
| `nano`  | Edit text                    |
| `vim`   | Edit text                    |


## File Type Recognition
### 1 .Detailed listing**
Long-format listing of your home directory and of `/dev`. Identify file vs directory vs "something else" from the first character of each permission string.

ls -l /home
total 4
drwxr-x--- 5 sysadmin sysadmin 4096 Sep 12 15:38 sysadmin

ls -l /dev
total 0
crw-r--r--  1 root     root     10, 235 Aug  9 11:26 autofs
drwxr-xr-x  2 root     root         320 Sep 12 15:49 block
drwxr-xr-x  2 root     root          80 Aug  9 11:26 bsg
crw-rw----  1 root     disk     10, 234 Aug  9 11:26 btrfs-control
drwxr-xr-x  3 root     root          60 Aug  9 11:26 bus
lrwxrwxrwx  1 root     root           3 Aug  9 11:26 cdrom -> sr0

hard link in Linux is **an additional filename that points directly to the exact same physical data and inode on your storage drive**

### 2 .Find a device file**
Locate at least one device file in `/dev`. Note whether it's block (`b`) or character (`c`), and explain conceptually what that distinction means.

crw-rw----+ 1 root     cdrom    21,   0 Aug  9 11:26 sg0

brw-rw----  1 root     disk      8,   1 Aug  9 11:26 sda1

|  | `c` Character | `b` Block |
| --- | --- | --- |
| Data | Stream of bytes | Blocks |
| Access | Sequential/stream-like | Random access |
| Typical use | Terminal, keyboard, `/dev/null` | HDD, SSD, USB |
| Example | `/dev/tty` | `/dev/sda` |

**special files that represent hardware or kernel devices**. They give programs a standard way to communicate with devices.

c → communicate with a device as a stream of bytes
b → access storage as blocks

### 3 .Symlink spotting**
Find at least one symbolic link (try `/etc` or `/usr/bin`). Note its `l` marker and what extra info its listing shows that a regular file's doesn't.

lrwxrwxrwx 1 root root         27 Aug  6 23:26 localtime -> /usr/share/zoneinfo/Etc/UTC

## Reflection Questions
Why a unified tree instead of drive letters? 
- Why a unified tree instead of drive letters? →

> Linux organizes resources by **where they are mounted in one tree**, not by assigning each storage device a separate drive letter.

This gives a more consistent path structure and makes storage easier to combine/manage.

unified tree : every file, folder, storage drive, and hardware device branches out from one single root directory
A drive letter is **a single letter from A to Z, usually followed by a colon, that an operating system assigns to a physical or logical storage device**

Why /etc vs /var/log as separate concerns?

**`/etc` = instructions**

**`/var/log` = history**

/etc
→ configuration
→ tells programs what to do

/var/log
→ changing runtime information
→ tells you what programs/system have done

What's really happening when you read /proc/cpuinfo?
it looks like you're reading a file, but **there isn't normally a real file stored on your disk containing your CPU information**.

Instead:

```
cat /proc/cpuinfo
       ↓
   /proc filesystem
       ↓
    Linux kernel
       ↓
  CPU information
       ↓
   displayed to you
```

The kernel **generates the content when you read it**.

So `/proc` is called a **pseudo-filesystem** (or virtual filesystem).

Why document a server's baseline before changing it?

We document the server's baseline so we know its original state before making changes. If something breaks or behaves differently afterward, we can compare the new state with the baseline, identify what changed, troubleshoot, and restore the original configuration if we have a proper backup.

Before change
↓
Document baseline
↓
Make modification
↓
Something goes wrong?
↓
Compare → identify changes → troubleshoot/restore