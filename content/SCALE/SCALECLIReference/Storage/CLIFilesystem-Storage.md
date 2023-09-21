---
title: "Filesystem (Storage)"
description: "Provides information about the storage filesystem namespace in the TrueNAS CLI. Includes command syntax and common commands."
weight: 25
aliases:
draft: false
tags:
- scaleclistorage
- scaleacls
---
{{< toc >}}


{{< include file="/_includes/CLIGuideWIP.md" >}}

## Filesystem Namespace
The **storage filesystem** has x commands, and is based on filesystem and acl management functions found in the SCALE API and web UI.
It provides access to storage filesystem methods through the **storage filesystem** commands.

## Filesystem Commands 
The following **storage filesystem** commands allow you to manage filesystem permissions and perform basic filesystem commands such as `mkdir` and `lidir`.

You can enter commands from the main CLI prompt or from the **dataset** namespace prompt.

### Interactive Argument Editor (TUI)

{{< include file="/_includes/CLI/HintInteractiveArgsEditor.md" >}}




### Get Command

This is content from the smtp article and will need to be rewritten for this one, but is an example use case for `get`:

To read the MIB file, download a copy to a shared dataset. Enter <code>storage filesystem get path="/usr/local/share/snmp/mibs/<em>FILENAME.ext</em>" > <em>PATH</em>/<em>FILENAME.ext</em></code>, where *FILENAME.ext* is the MIB file and *PATH* is the path to a dataset with a share configured (SMB, NFD, or iSCSI). For more information, see [`storage filesystem get`]({{< relref "CLIFilesystem-Storage.md #get-command" >}}).

{{< expand "Command Example" "v" >}}
```
storage filesystem get path="/usr/local/share/snmp/mibs/TRUENAS-MIB.txt" > /mnt/tank/test/TRUENAS-MIB.txt
[0%] ...
[100%] ...
[100%] Job output (0 bytes) saved at '/mnt/tank/test/TRUENAS-MIB.txt'
```
{{< /expand >}}

Log in to the share to access the copy.

```
storage filesystem get path="/mnt/tank/files"
Please, specify the output file name using ' > filename.ext'
```


### Get_Dosmod Command




{{< expand "Command Example" "v" >}}
```
storage filesystem get_dosmode path="/mnt/tank"
+----------+-------+
| readonly | false |
|   hidden | false |
|   system | false |
|  archive | true  |
|  reparse | false |
|  offline | false |
|   sparse | false |
+----------+-------+
```
{{< /expand >}}
{{< /expand >}}

### Get_Acl Command




{{< expand "Command Example" "v" >}}
```
storage filesystem getacl path="/mnt/tank" simplified= true resolve_ids= false
+---------+-----------+
|     uid | 0         |
|     gid | 0         |
|     acl | <list>    |
|   flags | <dict>    |
| acltype | POSIX1E   |
| trivial | true      |
|    path | /mnt/tank |
+---------+-----------+
[truenas]> storage filesystem getacl path="/mnt/tank" simplified= true resolve_ids=true
+---------+-----------+
|     uid | 0         |
|     gid | 0         |
|     acl | <list>    |
|   flags | <dict>    |
| acltype | POSIX1E   |
| trivial | true      |
|    path | /mnt/tank |
+---------+-----------+
[truenas]> storage filesystem getacl path="/mnt/tank" simplified=false  resolve_ids=false
+---------+-----------+
|     uid | 0         |
|     gid | 0         |
|     acl | <list>    |
|   flags | <dict>    |
| acltype | POSIX1E   |
| trivial | true      |
|    path | /mnt/tank |
+---------+-----------+
[truenas]> storage filesystem getacl path="/mnt/tank" simplified=false  resolve_ids=true
+---------+-----------+
|     uid | 0         |
|     gid | 0         |
|     acl | <list>    |
|   flags | <dict>    |
| acltype | POSIX1E   |
| trivial | true      |
|    path | /mnt/tank |
+---------+-----------+
```
{{< /expand >}}
{{< /expand >}}

### Listdir Command

{{< expand "Command Example" "v" >}}
```
storage filesystem listdir path="/mnt/tank/files"
+--------+------------------------+------------------------+-----------+------+-------+------+-----+-----+---------------+-----------+
| name   | path                   | realpath               | type      | size | mode  | acl  | uid | gid | is_mountpoint | is_ctldir |
+--------+------------------------+------------------------+-----------+------+-------+------+-----+-----+---------------+-----------+
| newdir | /mnt/tank/files/newdir | /mnt/tank/files/newdir | DIRECTORY | 2    | 16888 | true | 0   | 0   | false         | false     |
+--------+------------------------+------------------------+-----------+------+-------+------+-----+-----+---------------+-----------+
```
{{< /expand >}}
{{< /expand >}}

### Mkdir Command



{{< expand "Command Example" "v" >}}
```
storage filesystem mkdir path="/mnt/tank/files/newdir"
+----------+------------------------+
|     name | newdir                 |
|     path | /mnt/tank/files/newdir |
| realpath | /mnt/tank/files/newdir |
|     type | DIRECTORY              |
|     size | 2                      |
|     mode | 16888                  |
|      acl | true                   |
|      uid | 0                      |
|      gid | 0                      |
+----------+------------------------+
```
{{< /expand >}}
{{< /expand >}}


### Stat Command


{{< expand "Command Example" "v" >}}
```
storage filesystem stat path="/mnt/tank/files"
+---------------+-----------------+
|      realpath | /mnt/tank/files |
|          type | DIRECTORY       |
|          size | 3               |
|          mode | 16888           |
|           uid | 0               |
|           gid | 0               |
|         atime | 1695128671.0    |
|         mtime | 1695128850.0    |
|         ctime | 1695128850.0    |
|         btime | 1695128671.0    |
|           dev | 1048650         |
|         inode | 34              |
|         nlink | 3               |
| is_mountpoint | true            |
|     is_ctldir | false           |
|          user | root            |
|         group | root            |
|           acl | true            |
+---------------+-----------------+
```
{{< /expand >}}
{{< /expand >}}


### Statfs Command
Returns filesystem stats for path specified. 


{{< expand "Using the Stattfs Command" "v" >}}
#### Description
The `statfs` command has one required property, `path`.
`path` is the full /mnt/pool/dataset string. Use `storage dataset query` to list all dataset paths on the system.
Enter the property argument using the `=` delimiter to separate property and double-qoted value.
Enter the command string then press <kbd>Enter</kbd>.
The command returns a table with filesystem flags, block, file, and byte information for the path entered.

Paths on clustered volumes may be specifed with the path prefix CLUSTER:<volume name>. For example, to list directories in the directory 'data' in the clustered volume smb01, the path should be specified as CLUSTER:smb01/data.
Raises: CallError(ENOENT) - Path not found

#### Usage
From the CLI prompt, enter:

<code>storage filesystem statfs path="<i>/mnt/tank/files</i>"</code>

Where */mnt/tank/file* is the full mount path for the dataset.

{{< expand "Command Example" "v" >}}
```
storage filesystem statfs path="/mnt/tank/files"
+------------------+------------------+
|            flags | RW               |
|                  | NOATIME          |
|                  | XATTR            |
|                  | NFS4ACL          |
|                  | CASEINSENSITIVE  |
|           fstype | zfs              |
|           source | tank/files       |
|             dest | /mnt/tank/files  |
|        blocksize | 131072           |
|     total_blocks | 47019            |
|      free_blocks | 47018            |
|     avail_blocks | 47018            |
|            files | 12036783         |
|       free_files | 12036776         |
|         name_max | 255              |
|             fsid | 2482091823497756 |
|      total_bytes | 6162874368       |
|       free_bytes | 6162743296       |
|      avail_bytes | 6162743296       |
| total_blocks_str | 47019            |
|  free_blocks_str | 47018            |
| avail_blocks_str | 47018            |
|  total_bytes_str | 6162874368       |
|   free_bytes_str | 6162743296       |
|  avail_bytes_str | 6162743296       |
+------------------+------------------+
```
{{< /expand >}}
{{< /expand >}}



{{< taglist tag="scaleclistorage" limit="10" title="Related CLI Storage Articles" >}}
{{< taglist tag="scaleacls" limit="10" title="Related ACL Articles" >}}