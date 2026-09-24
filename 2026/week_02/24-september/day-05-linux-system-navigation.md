# Day 05 — Linux System Navigation

**Date:** 2026-09-24

## 🎯 Objective

Become comfortable navigating and inspecting a Linux system from the command line.

## 📚 Study

* Linux directory hierarchy
* Absolute and relative paths
* Navigation commands
* File and directory inspection
* Hidden files

## 📚 What I learned

* Linux has a single directory tree that starts at `/`. The standard layout is described in `man 7 hier`.
* `/` is the root of the filesystem, while `/root` is the home directory of the root user. They are not the same.
* `..` always means "one level up", so `/root/..` resolves to `/`.
* An **absolute path** starts with `/` and works from anywhere. A **relative path** is resolved from the current directory.
* `cd -` returns to the previous directory (`$OLDPWD`).
* Files and directories whose names start with `.` are hidden from a plain `ls`. Hidden is a naming convention, not a security feature.
* `ls -a` shows hidden entries including `.` and `..`, while `ls -A` shows hidden entries without `.` and `..`.
* `find ~ -maxdepth 2 -name '.*'` searches for hidden files and directories up to two levels below the home directory. The pattern must be quoted so the shell does not expand it before `find` receives it.
* `file` identifies what a file actually is, `stat` shows its full metadata and `du -sh` shows how much space it uses.
* `ls -ltr /var/log` shows the most recently modified log files at the end of the list.

Useful commands from today:

| Command | Purpose |
| ------- | ------- |
| `pwd` | Show the current directory |
| `cd` / `cd -` | Change directory / go back to the previous one |
| `ls -laF` | List all entries in long format with type indicators |
| `ls -ld` | Show the directory itself, not its contents |
| `realpath` | Resolve a path to its absolute form |
| `mkdir -p` | Create nested directories in one step |
| `file` | Identify the type of a file |
| `find -maxdepth -name` | Search for files by name, with a depth limit |
| `cat` / `cat >>` | Print a file / append text to it (end input with `Ctrl+D`) |
| `stat` | Show detailed file metadata |
| `du -sh` | Show the total size of a directory |

## 🔨 What I built

Explored the root directory and the home directory of the root user:

```bash
ls /
realpath /root/..
sudo ls -la /root
```

Created a practice directory structure and navigated it with absolute and relative paths:

```bash
mkdir -p ~/lab/day08/{docs,scripts/tmp,logs}
cd /home/borse/lab/day08/scripts/tmp && pwd
cd ../../docs && pwd
cd - && cd ~ && pwd
ls -ld ~/lab/day08/ && ls -lhF ~/lab/day08/
```

Created a hidden directory with a hidden file and worked with its content:

```bash
mkdir .ascuns
ls -laF
file .ascuns
cat >> .ascuns/.fisier-ascuns.md
cat .ascuns/.fisier-ascuns.md
```

Inspected files, directories and logs:

```bash
ls -a ~ && ls -A ~
find ~ -maxdepth 2 -name '.*' -not -name '.'
stat ~/.ascuns && du -sh ~/.ascuns
ls -ltr /var/log | tail -5
ls -l /etc | head
```

## 🧠 Problems & solutions

### Problem

I explored `/root/..` expecting to see the root user's directory, and `/root` looked empty.

### Solution

`realpath /root/..` returned `/`, which showed that `..` had taken me one level up to the filesystem root. The home directory of the root user is `/root`, and it only looked empty because it contains hidden files. `sudo ls -la /root` showed them (`.bashrc`, `.profile`, `.ssh`, ...).

### Problem

`ls -a ~ && ls -A ~` looked like it produced only one output.

### Solution

Both commands ran, but their outputs were printed one after the other without a separator. The first listing starts with `.` and `..`, the second one does not. Adding a separator makes the difference visible:

```bash
ls -a ~; echo "-----"; ls -A ~
```

## 💡 Key takeaway

Before running any command, I need to know where I am (`pwd`) and what I am looking at (`ls -la`, `file`, `stat`).

Hidden files are easy to miss, and many important configuration files are hidden (`.bashrc`, `.ssh/`, `.gitconfig`).

## 🔗 Resources

* [hier(7) — Linux directory hierarchy](https://man7.org/linux/man-pages/man7/hier.7.html)
* [Filesystem Hierarchy Standard 3.0](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)
* [path_resolution(7)](https://man7.org/linux/man-pages/man7/path_resolution.7.html)
* [ls(1)](https://man7.org/linux/man-pages/man1/ls.1.html)
* [find(1)](https://man7.org/linux/man-pages/man1/find.1.html)

## ➡️ Next

Continue with processes and services after a short break.
