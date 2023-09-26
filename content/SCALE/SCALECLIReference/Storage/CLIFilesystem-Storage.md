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

### Acl_Is_Trivial Command
The `acl_is_trivial` command returns true if the ACL can be fully expressed as a file mode without losing any access rules.

{{< expand "Using the Acl_Is_Trivial Command" "v" >}}
#### Description
The `acl_is_trivial` command has one required property, `path`.
`path` is the full */mnt/pool/dataset* string. Use `storage dataset query` to list all dataset paths on the system.
Enter the property argument using the `=` delimiter to separate property and double-qoted value.
Enter the command string then press <kbd>Enter</kbd>.
The command returns true if the ACL for the path can be fully exprssed as a file mode without losing any access rules.

You can specify paths on clustered volumes with the path prefix CLUSTER:<volume name>. 
For example, to list directories in the directory *data* in the clustered volume *smb01*, specifiy the path as **CLUSTER:*smb01*/*data***.

#### Usage
From the CLI prompt, enter:
<code>storage filesystem acl_is_trivial path="/mnt/<i>poolName/datasetName</i>"</code>

Where:
* *poolName* is the name of the root dataset (pool name).
* *datasetName* is the name of the dataset.

{{< expand "Command Example" "v" >}}
```
storage filesystem acl_is_trivial path="/mnt/tank/shares"
false
```
{{< /expand >}}
{{< /exapnd >}}

### Can_Access_As_User Command
The `can_access_as_user` command checks if the specified username is able to access path with specific permissions. 
Specify at least one permission (read/write/execute). 
{{< include file="/_includes/CLI/CLICommandWIP.md" >}}
<!-- commenting out until we can get the syntax or TUI to work 
{{< expand "Using the Can_Access_As_User Command" "v" >}}
#### Description
The `can_access_as_user` command has three required properties, `path`, `username`, and `permissions`.
`path` is the full */mnt/pool/dataset* string. Use `storage dataset query` to list all dataset paths on the system.
`username` is the name of a user. Use `account user query` to list all users on the system.
`permissions` has one of three values specified: `read`, `write`, or `execute` specified inside curly brackets `{}`. Default for each is `null`. Use `null` to not check that permission.
Enter the `path` and `username` property arguments using the `=` delimiter to separate property and double-qoted values. 
Use `--` to open the interactive argument editor to specify the `permissions` values for `read`, `write`, and `execute` as `true`, `false`, or `null`.
Enter the command string then press <kbd>Enter</kbd>.
The command returns true if the ACL for the path can be fully exprssed as a file mode without losing any access rules.

You can specify paths on clustered volumes with the path prefix CLUSTER:<volume name>. 
For example, to list directories in the directory *data* in the clustered volume *smb01*, specifiy the path as **CLUSTER:*smb01*/*data***.

#### Usage
From the CLI prompt, enter:
<code>storage filesystem acl_is_trivial path="/mnt/<i>poolName/datasetName</i>"</code>

Where:
* *poolName* is the name of the root dataset (pool name).
* *datasetName* is the name of the dataset.

{{< expand "Command Example" "v" >}}
```
storage filesystem acl_is_trivial path="/mnt/tank/shares"
false
```
{{< /expand >}}
{{< /exapnd >}} -->

### Chown Command
Use the `chown` command to change the owner or group of the file path specified.

{{< expand "Using the Chown Command" "v" >}}
#### Description
The `chown` command has one required property, `filesystem_ownership`.
`filesystem_ownership` has four property arguments, `path`, `uid`, `gid` and `options`.
See **Filesystem_Ownership Properties** below for details.
Enter the command string then press <kbd>Enter</kbd>.
Enter `--` after `filesystem_ownership` to open the interactive text editor.
The command returns percentage value for preparing and finishing the change in ownership.

{{< expand "Filesystem_Ownership Properties" "v" >}}
{{< truetable >}}
Enter property arguments as an array object inside the curly brackets `{}`, using the `:` to separate double-quoted property and value, and a comma to separate each argument.

| Property | Required | Description | Syntax Example |
|----------|----------|-------------|----------------|
| `path` | yes | Enter the full dataset */mnt/pool/dataset* string. Use `storage dataset query` to list all dataset paths on the system. Enter the argument using the `=` delimiter separating property and double-quoted value. | <code>"path":"/mnt/<i>poolName</i>/<i>datasetName</i>"</code> |
| `uid ` | *yes | Enter either the `uid` or `gid` property argument to specify ownership of the `path` entered. Use the `account user query` command locate the `uid` for the user. Enter the argument using the `=` delimiter separating property and value. | <code>"path":"</i>uid</i>"</code> |
| `gid ` | *no |Enter either the `uid` or `gid` property argument to specify ownership of the `path` entered. Use the `account group query` command locate the `gid` for the group. Enter the argument using the `=` delimiter separating property and value. | <code>"path":"</i>gid</i>"</code> |
| `options` | no | Work in Progress. `options` has two properties, `recursive` and `traverse`. Default value is `{}`. Set `recursive` to `true` to include children of the specified dataset. Set `traverse` to `true` to include mount points outside the specified dataset. | WIP |
{{< /truetable >}}
{{< /exapnd >}}

#### Usage
From the CLI prompt, enter:
<code>storage filesystem chown filesystem_properties={"path":"/mnt/<i>poolName/datasetName"</i>","uid":"<i>3002</i>"}</code>

Where:
* *poolName* is the name of the root dataset (pool name).
* *datasetName* is the name of the dataset.
* *3002* is the UID for the user you want to be the owner of the specified dataset.

{{< expand "Command Example" "v" >}}
```
storage filesystem chown filesystem_ownership= {"path":"/mnt/tank/files","uid":"3002"}
[0%] Preparing to change owner....
[100%] Finished changing owner...
```
{{< /expand >}}
{{< /exapnd >}}

### Get Command
Use `get` to run a job that copies the contents of an existing file on the system and writes that content to a new file this command creates on the system.

{{< hint type=warning >}}
You must be logged into the system as the root user to use the `storage filesystem get` command.
{{< /hint >}}
{{< expand "Using the Get Command" "v" >}}
#### Description
The `get` command has one required property, `path`.
`path` is the full */mnt/pool/dataset/filename.ext* string that includes the existing file. Use `storage dataset query` to list all dataset paths on the system.
Enter the property argument using the `=` delimiter to separate property and double-quoted value.
Enter the `>` character followed by the new file name and extension where the copied content gets written. 
Enter the command string then press <kbd>Enter</kbd>.
The command returns progress in Job percentage completed.

To find the new file, exit the SCALE CLI to the system CLI prompt and enter `ls` at the root prompt. 
If you did not specify the full path where the system should create the new file, the file is included in the output of the list command at the root user prompt. 
If you specified a new path to another directory, change to that directory and run the `ls` command.
To read the file from a dataset path with a share configured (SMB, NFD, or iSCSI), log into that share to access the file. 

#### Usage
From the CLI prompt, enter:
<code>storage filesystem get path="/mnt/<i>poolName/datasetName</i>/<i>filename.txt</i>" > /mnt/<i>poolName/datasetName/newfilename.txt</i></code>

Where:
* *poolName* is the name of the root dataset (pool name).
* *datasetName* is the name of the dataset.
* *filename.txt* is the name of an existing file in the dataset specified.
* *newfilename.txt* is the new file where contents from *filename.txt* get copied to.

{{< expand "Command Example" "v" >}}
```
storage filesystem get path="/mnt/tank/testfile.txt" > testfile2.txt
[0%] ...
[100%] ...
[100%] Job output (0 bytes) saved at '/mnt/tank/testfile2.txt'
```
{{< /expand >}}
{{< /exapnd >}} 

### Get_Dosmode Command
Use `get_dosmode` to view the dosmode properties (read only, hidden, system or archive, reparse, offline, and sparse) for the specified path.

{{< expand "Using the Get_Dosmode Command" "v" >}}
#### Description
The `get_dosmode` command has one required property, `path`.
`path` is the full */mnt/pool/dataset* string. Use `storage dataset query` to list all dataset paths on the system.
Enter the property argument using the `=` delimiter to separate property and double-qoted value.
Enter the command string then press <kbd>Enter</kbd>.
The command returns a list with readonly, hidden, system archive, reparse, offline, and sparse properties for the specified path.

#### Usage
From the CLI prompt, enter:
<code>storage filesystem get_dosmode path="/mnt/<i>poolName/datasetName</i>"</code>

Where:
* *poolName* is the name of the root dataset (pool name).
* *datasetName* is the name of the dataset.

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

### Getacl Command
Use the `getacl` command to identify the ACL for the specified dataset.

{{< expand "Using the Getacl Command" "v" >}}
#### Description
The `getacl` command has one required property, `path`, and two optional properties, `simplified` and `resolve_ids`.
`path` is the full */mnt/pool/dataset* string. Use `storage dataset query` to list all dataset paths on the system.

`simplified` effect depends on the ACL type of the underlying filesystem. 
If set to `true` and if an NFSv4, ACL simplified permissions and flags are returned for ACL entries where applicable. 
If set to `true` and if a POSIX1E ACl, this setting has no impact on returned ACL. 
`simplified` returns a shortened form of the ACL permset and flags where applicable. 
If permissions are simplified, then the `perms` object contains only a single `BASIC` key with a string describing the underlying permissions set.

`resolve_ids` adds an additional `who` key to each ACL entry, that converts the numeric id to a user name or group name. 
In the case of owner@ and group@ (NFSv4) or USER_OBJ and GROUP_OBJ (POSIX1E), st_uid or st_gid converted from stat() return for file. 
In the case of MASK (POSIX1E), OTHER (POSIX1E), everyone@ (NFSv4), key `who` included but set to null. 
In case of failure to resolve the id to a name, `who` is set to null. Only use this option if resolving ids to names is required.

Enter the property argument using the `=` delimiter to separate property and value.
Enter the command string then press <kbd>Enter</kbd>.
The command returns with the ACL for the specified dataset.

#### Usage
From the CLI prompt, enter:

<code>storage filesystem getacl path="/mnt/<i>poolName/datasetName</i>"</code>

Where:
* *poolName* is the name of the root dataset (pool name).
* *datasetName* is the name of the dataset.

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
```
{{< /expand >}}
{{< /expand >}}

### Is_Immutable Command
Use `is_immutable to verify if the dataset is immutable.

{{< expand "Using the Is_Immutable Command" "v" >}}
#### Description
The `is_immutable` command has one required property, `path`.
`path` is the full */mnt/pool/dataset* string. Use `storage dataset query` to list all dataset paths on the system.
Enter the property argument using the `=` delimiter to separate property and value.
Enter the command string then press <kbd>Enter</kbd>.
The command returns true if immutable, false if not.

#### Usage
From the CLI prompt, enter:

<code>storage filesystem is_immutable path="/mnt/<i>poolName/datasetName</i>"</code>

Where:
* *poolName* is the name of the root dataset (pool name).
* *datasetName* is the name of the dataset.

{{< expand "Command Example" "v" >}}
```
storage filesystem is_immutable path="/mnt/tank/files"
false
```
{{< /expand >}}
{{< /expand >}}

### Listdir Command
Use `listdir` to get the contents of a directory. 

Specify paths on clustered volumes with the path prefex CLUSTER:*volume name*. 
To list directories in the data directory in the cluster volume *smb01*, specify path CLUSER:*smb01/data*.

{{< expand "Using the Listdir Command" "v" >}}
#### Description
The `listdir` command has one required property, `path`.
`path` is the full */mnt/pool/dataset* string. Use `storage dataset query` to list all dataset paths on the system.
Enter the property argument using the `=` delimiter to separate property and value.
Enter the command string then press <kbd>Enter</kbd>.
The command returns a table with the name, path, absolute path of the entry (realpath), type as any of DIRECTORY|FILE|SYMLINK\OTHER, size, size of the entry (mode), file mode/permission (ACL), uid, user id of entry owner (gid), extended ACL is present on file (is_mountpointl), path is a mountpoint (is_ctldr), and path is within special .zfs directory.

#### Usage
From the CLI prompt, enter:

<code>storage filesystem listdir path="/mnt/<i>poolName/datasetName</i>"</code>

Where:
* *poolName* is the name of the root dataset (pool name).
* *datasetName* is the name of the dataset.

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
Use `mkdir` to create a directory in the path specified. 

{{< expand "Using the Listdir Command" "v" >}}
#### Description
The `mkdir` command has one required property, `path`.
`path` is the full */mnt/pool/dataset* string with the name of the new directory at the end of the path. 
Use `storage dataset query` to list all dataset paths on the system.
Enter the property argument using the `=` delimiter to separate property and double-quoted value.
Enter the command string then press <kbd>Enter</kbd>.
The command returns a table with the name, path, absolute path of the entry (realpath), type as any of DIRECTORY|FILE|SYMLINK\OTHER, size, size of the entry (mode), file mode/permission (ACL), uid, and user id of entry owner (gid).

#### Usage
From the CLI prompt, enter:

<code>storage filesystem mkdir path="/mnt/<i>poolName/datasetName/newDirName</i>"</code>

Where:
* *poolName* is the name of the root dataset (pool name).
* *datasetName* is the name of the dataset.
* *newDirName* is the name of the new directory to create.

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

### Put Command
Use `put` to run a job that puts or appends the contents of an existing file on the system to a new file this command creates on the system.

{{< include file="/_includes/CLI/CLICommandWIP.md" >}}
<!-- commenting out until we can get the syntax or TUI to work 
{{< hint type=warning >}}
You must be logged into the system as the root user to use the `storage filesystem get` command.
{{< /hint >}}
{{< expand "Using the Get Command" "v" >}}
#### Description
The `put` command has two required properties, `path` and `options`.
`path` is the full */mnt/pool/dataset/filename.ext* string that includes the existing file. 
Use `storage dataset query` to list all dataset paths on the system.
Enter the property argument using the `=` delimiter to separate property and double-quoted value.

`options` includes two property arguments, `append` and `mode`. 
`append` if set to `true` appends the file specified to the new file. 
`mode` sets the size of the entry. 
Enter the `options` property arguments inside the `{}`, using the `:` to sepearate double-quoted property and values, and each argument separated by a comma.
Enter `--` after the `path` argument to open  and use the interactive argument editor to enter `options` values.

To find the new file, exit the SCALE CLI to the system CLI prompt and enter `ls` at the root prompt. 
If you did not specify the full path where the system should create the new file, the file is included in the output of the list command at the root user prompt. 
If you specified a new path to another directory, change to that directory and run the `ls` command.
To read the file from a dataset path with a share configured (SMB, NFD, or iSCSI), log into that share to access the file. 

#### Usage
From the CLI prompt, enter:
<code>storage filesystem put path="<i>/mnt/tank/testfile.txt</i>" options={"append":"<i>true</i>","mode":"<i>33000</i>"}</code>

Where:
* *poolName* is the name of the root dataset (pool name).
* *datasetName* is the name of the dataset.
* *testfile.txt* is the name of an existing file in the dataset specified.
* *true* appends the file contents.
* *33000** is the size of the file (mode).

{{< expand "Command Example" "v" >}}
```
storage filesystem put path="/mnt/tank/testfile.txt" options={"append":"true","mode":"33000"}
Error: ValueError("Pipe 'input' is not open")
```
{{< /expand >}}
{{< /exapnd >}}  -->

<!-- commenting out until we can get input from eng whether we should expose these in the CLI 
### Set_Dosmode Command

{{< include file="/_includes/CLI/CLICommandWIP.md" >}}

{{< hint type=warning >}}
You must be logged into the system as the root user to use the `storage filesystem get` command.
{{< /hint >}}
{{< expand "Using the Get Command" "v" >}}
#### Description
The `put` command has two required properties, `path` and `options`.
`path` is the full */mnt/pool/dataset/filename.ext* string that includes the existing file. 
Use `storage dataset query` to list all dataset paths on the system.
Enter the property argument using the `=` delimiter to separate property and double-quoted value.

`options` includes two property arguments, `append` and `mode`. 
`append` if set to `true` appends the file specified to the new file. 
`mode` sets the size of the entry. 
Enter the `options` property arguments inside the `{}`, using the `:` to sepearate double-quoted property and values, and each argument separated by a comma.
Enter `--` after the `path` argument to open  and use the interactive argument editor to enter `options` values.

{{< expand "" "v" >}}
{{< truetable >}}
Set to `true` to enable the following properties.

| Property | Description| 
|----------|------------|
| `readonly` |  |
| `hidden` |  |
| `system` |  |
| `archive` |  |
| `reparse` |  |
|   offline` |  |
| `sparse` |  |
{{< /truetable >}}
{{< /expand >}}

#### Usage
From the CLI prompt, enter:
<code>storage filesystem setdosmode path="<i>/mnt/poolName/datasetName</i>" options={"readonly":"<i>true</i>"}</code>

Where:
* *poolName* is the name of the root dataset (pool name).
* *datasetName* is the name of the dataset.
* *true* makes the specfied dataset/file readonly.

{{< expand "Command Example" "v" >}}
```
storage filesystem setdosmode path="/mnt/tank/files" options={"readonly":"true"}
```
{{< /expand >}}
{{< /exapnd >}}  

### Set_Immutable Command

### Set_acl Command-->

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

You can specify paths on clustered volumes with the path prefix CLUSTER:<volume name>. 
For example, to list directories in the directory *data* in the clustered volume *smb01*, specifiy the path as **CLUSTER:*smb01*/*data***.
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
