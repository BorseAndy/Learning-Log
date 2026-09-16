# Linux File Permissions

Linux uses file permissions to control who can access files and directories and what they are allowed to do with them.

## 1. Permission Structure

When using:

```bash
ls -l
```

a file or directory is displayed in long format.

Example:

```text
drwxr-xr-x 2 borse borse 4096 Sep 16 15:30 test
```

The output can be broken down as:

```text
d rwx r-x r-x  2  borse  borse  4096  Sep 16 15:30  test
│ │   │   │    │    │      │      │          │      │
│ │   │   │    │    │      │      │          │      └─ name
│ │   │   │    │    │      │      │          └──────── date/time
│ │   │   │    │    │      │      └─────────────────── size
│ │   │   │    │    │      └────────────────────────── group
│ │   │   │    │    └──────────────────────────────── owner
│ │   │   │    └───────────────────────────────────── hard links
│ │   │   └────────────────────────────────────────── others
│ │   └────────────────────────────────────────────── group
│ └────────────────────────────────────────────────── owner
└──────────────────────────────────────────────────── type
```

The first character identifies the file type:

| Character | Meaning      |
| --------- | ------------ |
| `-`       | Regular file |
| `d`       | Directory    |

The next nine characters represent permissions divided into three groups:

```text
rwx r-x r-x
│   │   │
│   │   └── others
│   └────── group
└────────── owner
```

Each group contains:

```text
r w x
│ │ │
│ │ └── execute
│ └──── write
└────── read
```

A `-` means that the corresponding permission is not granted.

---