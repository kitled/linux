# `zpool`

The `zpool` utility controls the operation of the pool and allows adding, removing, replacing, and managing disks.

> [!Note]
> To manage datasets and volumes, use [`zfs`](zfs.md).


## Overview

⏩ `Handbook` https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zpool

> [!Important]
> If you already have pools setup in prior installations, skip to Import.


## `create`

Examples.

### Using full disks

Create a simple `mirror` pool:

```sh
sudo zpool create mypool mirror /dev/sda dev/sdb
```

To create more than one `vdev` with a single command, specify groups of disks separated by the `vdev` type keyword, `mirror` in this example:

```sh
sudo zpool create mypool mirror \
/dev/sda /dev/sdb mirror /dev/sdc /dev/sdd
```

### Using partitions

Pools can also use partitions rather than whole disks. Putting ZFS in a separate partition allows the same disk to have other partitions for other purposes. In particular, it allows adding partitions with bootcode and file systems needed for booting. This allows booting from disks that are also members of a pool. ZFS adds no performance penalty on FreeBSD when using a partition rather than a whole disk. Using partitions also allows the administrator to *underprovision* the disks, using less than the full capacity. If a future replacement disk of the same nominal size as the original actually has a slightly smaller capacity, the smaller partition will still fit, using the replacement disk.

Create a [RAID-Z2](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-term-vdev-raidz) pool using partitions:

```sh
sudo zpool create mypool raidz2 \
/dev/sda1 /dev/sdb1 /dev/sdc1 /dev/sdd1 \
/dev/sde1 /dev/sdf1
```





## `scrub`

Routinely [scrub](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-term-scrub) pools, ideally at least once every month. The `scrub` operation is disk-intensive and will reduce performance while running. Avoid high-demand periods when scheduling `scrub` or use `vfs.zfs.scrub_delay` to adjust the relative priority of the `scrub` to keep it from slowing down other workloads.

1. Run `scrub` on a given pool.

    ```
    sudo zpool scrub mypool
    ```

1. Check status.

    ```
    sudo zpool status
    ```

    ▼ 🖥️`stdout`

    ```
      pool: mypool
     state: ONLINE
      scan: scrub in progress since Wed Feb 19 20:52:54 2014
            116G scanned out of 8.60T at 649M/s, 3h48m to go
            0 repaired, 1.32% done
    config:

            NAME        STATE     READ WRITE CKSUM
            mypool      ONLINE       0     0     0
              raidz2-0  ONLINE       0     0     0
                ada0p3  ONLINE       0     0     0
                ada1p3  ONLINE       0     0     0
                ada2p3  ONLINE       0     0     0
                ada3p3  ONLINE       0     0     0
                ada4p3  ONLINE       0     0     0
                ada5p3  ONLINE       0     0     0

    errors: No known data errors
    ```



## `export` & `import`

⏩ https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zpool-import

### `export`

Export pools before moving them to another system.

```sh
sudo zpool export mypool
```

### `import`

Importing a pool automatically mounts the datasets. 

1. List all available pools for import.

    ```sh
    sudo zpool import
    ```

    ▼ 🖥️`stdout`

    ```
       pool: mypool
         id: 9930174748043525076
      state: ONLINE
     action: The pool can be imported using its name or numeric identifier.
     config:

            mypool      ONLINE
              ada2p3    ONLINE
    ```

1. Import the pool, optionally with an **alt**ernative **root** directory.

    ```sh
    sudo zpool import -o altroot=/mnt mypool
    ```

1. Check that it's properly mounted.

    ```sh
    sudo zfs list
    ```

    ▼ 🖥️`stdout`

    ```
    zfs list
    NAME                 USED  AVAIL  REFER  MOUNTPOINT
    mypool               110K  47.0G    31K  /mnt/mypool
    ```



## `upgrade`

Upgrading is a one-way process.

Upgrade a v28 pool to support `Feature Flags`:

1. Check `status` to see if pools may be updated.

    ```sh
    sudo zpool status
    ```
    ▼ 🖥️`stdout`
    ```
      pool: mypool
     state: ONLINE
    status: The pool is formatted using a legacy on-disk format.  The pool can
            still be used, but some features are unavailable.
    action: Upgrade the pool using 'zpool upgrade'.  Once this is done, the
            pool will no longer be accessible on software that does not support feat
            flags.
      scan: none requested
    config:

            NAME        STATE     READ WRITE CKSUM
            mypool      ONLINE       0     0     0
              mirror-0  ONLINE       0     0     0
    	    ada0    ONLINE       0     0     0
    	    ada1    ONLINE       0     0     0

    errors: No known data errors
    ```

1. Query `zpool upgrade`

    ```
    sudo zpool upgrade
    ```
    ▼ 🖥️`stdout`
    ```
    This system supports ZFS pool feature flags.

    The following pools are formatted with legacy version numbers and are upgraded to use feature flags.
    After being upgraded, these pools will no longer be accessible by software that does not support feature flags.

    VER  POOL
    ---  ------------
    28   mypool

    Use 'zpool upgrade -v' for a list of available legacy versions.
    Every feature flags pool has all supported features enabled.
    ```

1. Do the `upgrade`.

    ```
    sudo zpool upgrade mypool
    ```
    ▼ 🖥️`stdout`
    ```
    This system supports ZFS pool feature flags.

    Successfully upgraded 'mypool' from version 28 to feature flags.
    Enabled the following features on 'mypool':
      async_destroy
      empty_bpobj
      lz4_compress
      multi_vdev_crash_dump
    ```








