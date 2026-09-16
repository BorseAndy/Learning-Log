# Linux Users and Groups

Linux uses users and groups as part of its access-control model. Files and directories have an owner and a group, and permissions determine what users can do with those resources.

## 1. Linux Users

Every Linux user account has a **UID (User ID)**.

The UID is a numeric identifier assigned to the user.

For example:

```text
uid=1000(borse)
```

means:

* username: `borse`
* UID: `1000`

The UID is an **identifier**, not a permission level.

A higher UID does not mean more or fewer permissions.

---

## 2. Linux Groups

Groups have a **GID (Group ID)**.

A user can belong to multiple groups.

The command:

```bash
id
```

displays information about the current user.

Example:

```text
uid=1000(borse)
gid=1000(borse)
groups=1000(borse),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),100(users)
```

This shows:

* the user's UID;
* the user's primary GID;
* the groups the user belongs to.

---

## 3. Primary Group and Supplementary Groups

The `gid` shown by `id` represents the user's **primary group**.

For example:

```text
gid=1000(borse)
```

The `groups` section lists all groups the user belongs to:

```text
groups=1000(borse),4(adm),24(cdrom),27(sudo),...
```

Group membership can provide additional permissions depending on the system configuration and the resources being accessed.

---

## 4. Group Membership and Permissions

A user's group membership can affect which permission set applies when accessing a file or directory.

For example, a file might have:

```text
-rw-r-----
```

with:

```text
owner: borse
group: developers
```

The permissions are:

```text
rw- | r-- | ---
owner | group | others
```

A member of the `developers` group would be evaluated against the **group permissions** for that file.

This is one of the reasons groups are useful: permissions can be granted to multiple users through group membership instead of configuring every user individually.

---

## 5. The `sudo` Group

On systems configured this way, membership in the `sudo` group allows a user to use `sudo` according to the system's `sudo` configuration.

For example:

```bash
sudo <command>
```

runs the command with elevated privileges when the user is authorized to do so.

Group membership itself is therefore not a universal permission system; its effect depends on the system's configuration.

---

## 6. Running a Command as Another User

The `sudo -u` option can execute a command as a specific user.

For example:

```bash
sudo -u postgres id
```

runs:

```bash
id
```

as the Linux user:

```text
postgres
```

This does **not** permanently switch the current shell/user.

It is useful when testing what another user can access or when working with services that run under dedicated accounts.

---

## 7. System Users

Linux systems commonly use dedicated users for services.

For example:

```text
postgres
```

is typically a dedicated Linux system user associated with the PostgreSQL service.

This user is separate from the normal interactive user account:

```text
borse
```

A service running under its own user account can therefore operate with its own ownership and permissions rather than running everything as a normal user.

---

## 8. Creating a User

A new user was created during Day 03 with:

```bash
sudo useradd -m -s /bin/bash -c "Test-User" test-user-linux
```

The options used were:

| Option            | Meaning                               |
| ----------------- | ------------------------------------- |
| `-m`              | Create the user's home directory      |
| `-s /bin/bash`    | Set `/bin/bash` as the login shell    |
| `-c "Test-User"`  | Set the account's comment/GECOS field |
| `test-user-linux` | Username                              |

The account password can then be configured using:

```bash
passwd
```

---

## 9. Users, Groups and File Ownership

Files and directories have an owner and a group.

For example:

```text
-rwxr----- 1 borse borse 1644 Sep 16 16:06 testfile.md
```

The relevant ownership information is:

```text
owner: borse
group: borse
```

The permission structure is:

```text
rwx | r-- | ---
owner | group | others
```

When a user accesses the file, Linux uses the user's identity, group membership, ownership and permission bits to determine the applicable access.

---

## 10. Changing Ownership

The `chown` command changes ownership.

Change the owner:

```bash
sudo chown test-user-linux test
```

This changes the owner of `test` to:

```text
test-user-linux
```

while leaving the group unchanged.

To change both owner and group:

```bash
sudo chown borse:test-user-linux test/
```

The general form is:

```bash
chown <owner>:<group> <file-or-directory>
```

---

## 11. Permission Problems and Ownership

Changing the owner or group of a directory can affect access to files inside it.

During Day 03, changing the ownership/permissions of the `test` directory resulted in:

```bash
ls -l test/testfile.md
```

returning:

```text
ls: cannot access 'test/testfile.md': Permission denied
```

Using:

```bash
sudo ls -l test/testfile.md
```

worked because the command was executed with elevated privileges.

The experiment demonstrated that access to a file depends not only on the file's own permissions, but also on the permissions required to traverse its parent directories.

---

## 12. Useful Commands

### Display current user and groups

```bash
id
```

### Run a command as another user

```bash
sudo -u <user> <command>
```

Example:

```bash
sudo -u postgres id
```

### Create a user

```bash
sudo useradd -m -s /bin/bash -c "Comment" username
```

### Set/change a password

```bash
passwd
```

### Change file owner

```bash
sudo chown user file
```

### Change owner and group

```bash
sudo chown user:group file
```

---

## Key Concepts

```text
USER
 │
 ├── UID
 │
 └── GROUP MEMBERSHIP
       │
       ├── primary group → GID
       └── supplementary groups
                │
                ↓
        affects access to resources

FILE / DIRECTORY
 │
 ├── owner
 ├── group
 └── permissions
       ├── owner
       ├── group
       └── others
```

The main idea is:

> **Users identify who is accessing a resource, groups allow permissions to be shared among users, ownership identifies the associated user/group, and permissions define what each class can do.**