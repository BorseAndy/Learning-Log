# Day 03 — Linux Users and Permissions

**Date:** 2026-09-16

## 🎯 Objective

Understand Linux users, groups, ownership and file permissions.

## 📚 What I learned

* Linux users have a **UID (User ID)** and groups have a **GID (Group ID)**.
* `id` displays the current user's UID, primary GID and group memberships.
* UID/GID values are **identifiers**, not permission levels.
* Group membership can provide additional permissions depending on the system configuration.
* `sudo -u <user> <command>` runs a command as another Linux user without permanently switching users.
* `ls -l` displays file type, permissions, owner, group, size and modification time.
* File permissions are divided into **owner / group / others** and use `r` (read), `w` (write) and `x` (execute).

## 🔨 What I practiced

### User management

Created a new Linux user:

```bash
sudo useradd -m -s /bin/bash -c "Test-User" test-user-linux
```

Also practiced setting the account password with:

```bash
passwd
```

### File permissions

Created a test directory and file and inspected their permissions using:

```bash
ls -l
```

Practiced modifying permissions with symbolic notation:

```bash
chmod g-x test/
```

and numeric notation:

```bash
chmod 755 testfile.md
chmod 640 testfile.md
chmod 740 testfile.md
```

This helped me understand how the three permission groups (**owner / group / others**) are represented and modified.

### Ownership

Changed the owner of a directory using:

```bash
sudo chown test-user-linux test
```

and later changed the owner/group combination with:

```bash
sudo chown borse:test-user-linux test/
```

## 🧠 Problem & solution

After changing the **owner** of the `test` directory, I could no longer access:

```bash
ls -l test/testfile.md
```

and received:

```text
Permission denied
```

I solved the immediate problem with:

```bash
sudo ls -l test/testfile.md
```

I also restored the directory ownership/group relationship with:

```bash
sudo chown borse:test-user-linux test/
```

### What I learned from the problem

Changing ownership or permissions on a **directory** can affect whether I can access files inside it. The permissions of the directory itself therefore matter when accessing its contents.

## 💡 Key takeaway

Linux access control is based on the combination of **user identity, group membership, ownership and permissions**. `ls -l`, `chmod` and `chown` are essential tools for inspecting and modifying these properties.

## 🔗 Resource

* [Linux Journey — Permissions](https://labex.io/linuxjourney/courses/permissions)

## ➡️ Next

Continue with the next topic in the DevOps learning roadmap.