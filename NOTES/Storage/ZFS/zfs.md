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

To see all the commands available in ZFS, see `zfs(8)` or `zpool(8)`.

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









