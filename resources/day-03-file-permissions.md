## 1. Basic Permissions

### Read — `r`

Allows reading the contents of a file.

For a directory, read permission allows viewing the directory entries.

### Write — `w`

Allows modifying the contents of a file.

For a directory, write permission is related to creating, deleting and renaming entries within the directory.

### Execute — `x`

For a file, execute permission allows the file to be executed as a program/script.

For a directory, execute permission allows the directory to be traversed/accessed.

---

## 2. Permission Classes

Permissions are assigned independently to three classes:

```text
owner | group | others
```

For example:

```text
rwx r-x r--
```

means:

* **owner:** `rwx`
* **group:** `r-x`
* **others:** `r--`

These permissions are evaluated according to the user accessing the resource and the ownership/group relationship of that resource.

---

## 3. Symbolic Permission Notation

Permissions can be modified using `chmod`.

For example:

```bash
chmod g-x test/
```

Here:

* `g` = group
* `-x` = remove execute permission

Other useful permission targets include:

```text
u = owner/user
g = group
o = others
a = all
```

Examples:

```bash
chmod u+x script.sh
chmod g-w file.txt
chmod o-r file.txt
chmod a+x script.sh
```

---

## 4. Octal Permission Notation

Permissions can also be represented using numbers.

The basic values are:

| Permission | Value |
| ---------- | ----: |
| `r`        |     4 |
| `w`        |     2 |
| `x`        |     1 |
| `-`        |     0 |

The values are added together within each permission class.

### Common combinations

| Octal | Permissions |
| ----: | ----------- |
|   `7` | `rwx`       |
|   `6` | `rw-`       |
|   `5` | `r-x`       |
|   `4` | `r--`       |
|   `3` | `-wx`       |
|   `2` | `-w-`       |
|   `1` | `--x`       |
|   `0` | `---`       |

A three-digit octal mode represents:

```text
owner | group | others
```

For example:

```bash
chmod 755 file
```

means:

```text
7 → owner  → rwx
5 → group  → r-x
5 → others → r-x
```

Result:

```text
rwxr-xr-x
```

Another example:

```bash
chmod 640 file
```

means:

```text
6 → owner  → rw-
4 → group  → r--
0 → others → ---
```

Result:

```text
rw-r-----
```

---

## 5. Practical Examples

During Day 03, the following permission changes were practiced:

```bash
chmod 755 testfile.md
chmod 640 testfile.md
chmod 740 testfile.md
```

The resulting permissions were:

```text
755 → rwxr-xr-x
640 → rw-r-----
740 → rwxr-----
```

This demonstrated how changing the numeric mode changes the permissions assigned to the owner, group and others.

---

## 6. Directory Permissions

Directory permissions are particularly important because they affect access to the files and directories inside them.

For example, a directory may have:

```text
drwxr-xr-x
```

while another may have:

```text
drwxr--r--
```

Removing execute permission from a directory can prevent a user from accessing/traversing paths inside that directory, even when the files themselves have permissions that would otherwise allow access.

This was demonstrated during Day 03 when changing the permissions/ownership of the `test` directory resulted in:

```text
Permission denied
```

when attempting to access a file inside it.

---

## 7. Changing Ownership

Ownership is separate from the permission bits.

`chown` is used to change the owner and/or group of a file or directory.

Change the owner:

```bash
sudo chown test-user-linux test
```

Change both owner and group:

```bash
sudo chown borse:test-user-linux test/
```

The general syntax is:

```bash
chown [owner][:group] <file-or-directory>
```

Ownership and permissions work together to determine access to a resource.

---

## 8. Useful Commands

### Inspect permissions

```bash
ls -l
```

### Change permissions

```bash
chmod <mode> <file>
```

Examples:

```bash
chmod 755 script.sh
chmod 640 config.txt
```

### Change permissions symbolically

```bash
chmod g-x directory/
chmod u+x script.sh
```

### Change ownership

```bash
sudo chown user file
sudo chown user:group file
```

---

## Key Concepts

```text
ls -l
   ↓
inspect permissions and ownership

chmod
   ↓
change permissions

chown
   ↓
change ownership/group

permissions
   ↓
owner | group | others
   ↓
read | write | execute
```

The important distinction is:

> **Ownership determines which user/group a permission set applies to; permission bits determine what that user/group can do.**
