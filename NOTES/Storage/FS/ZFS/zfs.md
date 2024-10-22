# `zfs`

The `zfs` utility can create, destroy, and manage all existing ZFS datasets within a pool.

> [!Note]
> To manage the pool itself, use [`zpool`](zpool.md).

## Overview

⏩ `Handbook` https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zfs





## `create` & `destroy`

### Datasets

#### Canonical command

To create a dataset, use:

```sh
sudo zfs create <nameofzpool>/<nameofdataset>
```

It is then possible to apply ZFS specific attributes to the dataset.  
For example, one could assign a quota limit to a specific directory within a dataset:

```sh
sudo zfs set quota=20G <nameofzpool>/<nameofdataset>/<directory>
```




#### Examples

1. To create a dataset named `ds0` in the pool named `tank`, do:

   ```sh
   sudo zfs create tank/ds0
   ```

   You can nest datasets.
      
   However, each dataset is managed (snapshot, shared, …) on its own.  
   Nesting thus complexifies maintenance.

1. Inspect datasets. This helps keeping track of nesting.

   ```sh
   sudo zfs list
   ```

1. Inspect properties, either all or some in particular.

   ```sh
   sudo zfs get all ds0          # see all properties    only for "ds0"
   sudo zfs get sharenfs         # see only "sharenfs"   for all datasets
   sudo zfs get mountpoint ds0   # see only "mountpoint" only for "ds0"
   ```




#### Detail

⏩ https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zfs-create

Unlike traditional disks and volume managers, space in ZFS is *not* preallocated. With traditional file systems, after partitioning and assigning the space, there is no way to add a new file system without adding a new disk. With ZFS, creating new file systems is possible at any time. Each [*dataset*](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-term-dataset) has properties including features like compression, deduplication, caching, and quotas, as well as other useful properties like readonly, case sensitivity, network file sharing, and a mount point. Nesting datasets within each other is possible and child datasets will inherit properties from their ancestors. [Delegate](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zfs-allow), [replicate](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zfs-send), [snapshot](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zfs-snapshot), [jail](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zfs-jail) allows administering and destroying each dataset as a unit. Creating a separate dataset for each different type or set of files has advantages. The drawbacks to having a large number of datasets are that some commands like `zfs list` will be slower, and that mounting of hundreds or even thousands of datasets will slow the FreeBSD boot process.

1. Create a new dataset and enable [LZ4 compression](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-term-compression-lz4) on it:

    ```sh
    sudo zfs create -o compress=lz4 mypool/usr/mydataset
    ```

1. Destroy the created dataset:

    ```sh
    sudo zfs destroy mypool/usr/mydataset
    ```

In modern versions of ZFS, `zfs destroy` is asynchronous, and the free space might take minutes to appear in the pool. Use `zpool get freeing poolname` to see the `freeing` property, that shows which datasets are having their blocks freed in the background. If there are child datasets, like [snapshots](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-term-snapshot) or other datasets, destroying the parent is impossible. To destroy a dataset and its children, use `-r` to recursively destroy the dataset and its children. Use `-n -v` to list datasets and snapshots destroyed by this operation, without actually destroy anything. Space reclaimed by destroying snapshots is also shown.





### Volumes (zvol)

⏩ https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zfs-volume

A volume is a special dataset type. Rather than mounting as a file system, expose it as a block device under `/dev/zvol/poolname/dataset`. This allows using the volume for other file systems, to back the disks of a virtual machine, or to make it available to other network hosts using protocols like iSCSI or HAST.




## `snapshot`

⏩ https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zfs-snapshot

> A snapshot provides a read-only, point-in-time copy of the dataset.  
A snapshot from a dataset duplicates everything contained in it. This includes the file system properties, files, directories, permissions, and so on. Snapshots use no extra space when first created, but consume space as the blocks they reference change.  
Recursive snapshots taken with `-r` create snapshots with the same name on the dataset and its children, providing a consistent moment-in-time snapshot of the file systems. This can be important when an application has files on related datasets or that depend upon each other. Without snapshots, a backup would have copies of the files from different points in time.
> 
> A typical example of snapshot use is as a quick way of backing up the current state of the file system when performing a risky action like a software installation or a system upgrade. If the action fails, rolling back to the snapshot returns the system to the same state when creating the snapshot. If the upgrade was successful, delete the snapshot to free up space. Without snapshots, a failed upgrade often requires restoring backups, which is tedious, time consuming, and may require downtime during which the system is unusable. Rolling back to snapshots is fast, even while the system is running in normal operation, with little or no downtime. The time savings are enormous with multi-terabyte storage systems considering the time required to copy the data from backup. Snapshots are not a replacement for a complete backup of a pool, but offer a quick and easy way to store a dataset copy at a specific time.




### Create snapshots

⏩ https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zfs-snapshot-creation

To create snapshots, use zfs snapshot dataset@snapshotname. Adding -r creates a snapshot recursively, with the same name on all child datasets.


```sh
sudo zfs list -t all
sudo zfs snapshot -r mypool@my_recursive_snapshot
sudo zfs list -t snapshot
```

Snapshots are not shown by a normal `zfs list` operation. To list snapshots, append `-t snapshot` to `zfs list`. `-t all` displays both file systems and snapshots.




### Comparing snapshots

⏩ https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zfs-snapshot-diff

ZFS provides a built-in command to compare the differences in content between two snapshots. This is helpful with a lot of snapshots taken over time when the user wants to see how the file system has changed over time. For example, `zfs diff` lets a user find the latest snapshot that still contains a file deleted by accident. Doing this for the two snapshots created in the previous section yields this output:

```sh
sudo zfs list -rt all mypool/var/tmp
sudo zfs diff mypool/var/tmp@my_recursive_snapshot
```

The command lists the changes between the specified snapshot (in this case `mypool/var/tmp@my_recursive_snapshot`) and the live file system. The first column shows the change type:

| Sym | Meaning
| - | ---
| + | Adding the path or file.
| - | Deleting the path or file.
| M | Modifying the path or file.
| R | Renaming the path or file.

Comparing the output with the table, it becomes clear that ZFS added **passwd** after creating the snapshot `mypool/var/tmp@my_recursive_snapshot`. This also resulted in a modification to the parent directory mounted at `/var/tmp`.

Comparing two snapshots is helpful when using the ZFS replication feature to transfer a dataset to a different host for backup purposes.

Compare two snapshots by providing the full dataset name and snapshot name of both datasets:

```sh
sudo cp /var/tmp/passwd /var/tmp/passwd.copy
sudo zfs snapshot mypool/var/tmp@diff_snapshot
sudo zfs diff mypool/var/tmp@my_recursive_snapshot mypool/var/tmp@diff_snapshot
```

```
M       /var/tmp/
+       /var/tmp/passwd
+       /var/tmp/passwd.copy
```

```sh
sudo zfs diff mypool/var/tmp@my_recursive_snapshot mypool/var/tmp@after_cp
```

```
M       /var/tmp/
+       /var/tmp/passwd
```

A backup administrator can compare two snapshots received from the sending host and determine the actual changes in the dataset. See the [Replication](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zfs-send) section for more information.




### Rollback

⏩ https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zfs-snapshot-rollback

zfs list -rt all mypool/var/tmp
ls /var/tmp
rm /var/tmp/passwd*
ls /var/tmp
zfs rollback mypool/var/tmp@diff_snapshot
ls /var/tmp


zfs list -rt snapshot mypool/var/tmp

This warning means that snapshots exist between the current state of the dataset and the snapshot to which the user wants to roll back. To complete the rollback delete these snapshots. 

zfs rollback -r mypool/var/tmp@my_recursive_snapshot
zfs list -rt snapshot mypool/var/tmp

### Restoring Individual Files

⏩ https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zfs-snapshot-snapdir

Snapshots live in a hidden directory under the parent dataset: `.zfs/snapshots/snapshotname`. By default, these directories will not show even when executing a standard `ls -a` . Although the directory doesn’t show, access it like any normal directory. The property named `snapdir` controls whether these hidden directories show up in a directory listing. Setting the property to `visible` allows them to appear in the output of `ls` and other commands that deal with directory contents.

```sh
# zfs get snapdir mypool/var/tmp
NAME            PROPERTY  VALUE    SOURCE
mypool/var/tmp  snapdir   hidden   default
# ls -a /var/tmp
.               ..              passwd          vi.recover
# zfs set snapdir=visible mypool/var/tmp
# ls -a /var/tmp
.               ..              .zfs            passwd          vi.recover
```

Restore individual files to a previous state by copying them from the snapshot back to the parent dataset. The directory structure below `.zfs/snapshot` has a directory named like the snapshots taken earlier to make it easier to identify them. The next example shows how to restore a file from the hidden `.zfs` directory by copying it from the snapshot containing the latest version of the file:

```sh
# rm /var/tmp/passwd
# ls -a /var/tmp
.               ..              .zfs            vi.recover
# ls /var/tmp/.zfs/snapshot
after_cp                my_recursive_snapshot
# ls /var/tmp/.zfs/snapshot/after_cp
passwd          vi.recover
# cp /var/tmp/.zfs/snapshot/after_cp/passwd /var/tmp
```

Even if the snapdir property is set to hidden, running ls .zfs/snapshot will still list the contents of that directory. The administrator decides whether to display these directories. This is a per-dataset setting. Copying files or directories from this hidden .zfs/snapshot is simple enough. Trying it the other way around results in this error:

```sh
# cp /etc/rc.conf /var/tmp/.zfs/snapshot/after_cp/
cp: /var/tmp/.zfs/snapshot/after_cp/rc.conf: Read-only file system
```

The error reminds the user that snapshots are read-only and cannot change after creation. Copying files into and removing them from snapshot directories are both disallowed because that would change the state of the dataset they represent.

Snapshots consume space based on how much the parent file system has changed since the time of the snapshot. The `written` property of a snapshot tracks the space the snapshot uses.

To destroy snapshots and reclaim the space, use `zfs destroy dataset@snapshot`. Adding `-r` recursively removes all snapshots with the same name under the parent dataset. Adding `-n -v` to the command displays a list of the snapshots to be deleted and an estimate of the space it would reclaim without performing the actual destroy operation.








## `send` & `receive`

🅰️ https://wiki.archlinux.org/title/ZFS#Transmitting_snapshots_with_ZFS_Send_and_ZFS_Recv

### Locally

🅰️ https://wiki.archlinux.org/title/ZFS#Basic_ZFS_Send

First, create a snapshot of some ZFS filesystem:

```sh
sudo zfs snapshot zpool0/archive/books@snap
```

Now send the snapshot to a new location on a different zpool:

```sh
sudo zfs send -v zpool0/archive/books@snap | zfs recv zpool4/library
```

The contents of zpool0/archive/books@snap are now live at zpool4/library

> [!Tip]
> See [man zfs-send][man-zfs-send] and [man zfs-recv][man-zfs-recv] for details on flags.

### Over SSH

🅰️ https://wiki.archlinux.org/title/ZFS#Send_over_ssh

Use the ZFS Delegation system to allow a non-root user on each system to perform the respective send and receive operations. 

> [!Important]
> You may want to setup [`ssh`](../SSH/config.md) first.


On the sending system:

```sh
sudo zfs allow -u someuser send,snapshot mypool
```

On the receiving system:

```
# sysctl vfs.usermount=1
vfs.usermount: 0 -> 1
# echo vfs.usermount=1 >> /etc/sysctl.conf
# zfs create recvpool/backup
# zfs allow -u someuser create,mount,receive recvpool/backup
# chown someuser /recvpool/backup
```

The unprivileged user can receive and mount datasets now, and replicates the home dataset to the remote system:

```
% zfs snapshot -r mypool/home@monday
% zfs send -R mypool/home@monday | ssh someuser@backuphost zfs recv -dvu recvpool/backup
```

Create a recursive snapshot called monday of the file system dataset home on the pool mypool. Then zfs send -R includes the dataset, all child datasets, snapshots, clones, and settings in the stream. Pipe the output through SSH to the waiting zfs receive on the remote host backuphost. Using an IP address or fully qualified domain name is good practice. The receiving machine writes the data to the backup dataset on the recvpool pool. Adding -d to zfs recv overwrites the name of the pool on the receiving side with the name of the snapshot. -u causes the file systems to not mount on the receiving side. Using -v shows more details about the transfer, including the elapsed time and the amount of data transferred.








[man-zfs-send]: https://openzfs.github.io/openzfs-docs/man/8/zfs-send.8.html
[man-zfs-recv]: https://openzfs.github.io/openzfs-docs/man/8/zfs-recv.8.html
