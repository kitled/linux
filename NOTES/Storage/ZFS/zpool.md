# `zpool`

The `zpool` utility controls the operation of the pool and allows adding, removing, replacing, and managing disks.

> [!Note]
> To manage datasets and volumes, use [`zfs`](zfs.md).





## Overview

⏩ `Handbook` https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zpool

> [!Important]
> Skip to [`import`](#import) if you already have pools from prior installations.





## `create`

> [!Important]
> Make sure you have [persistent block device names](../names.md#persistent-block-device-naming) for the relevant disks or partitions.
>
> ```sh
> ls -l /dev/disk/by-id/          # for disks
> ls -l /dev/disk/by-partlabel/   # for partitions
> ```




### Using full disks

To create a ZFS pool:

```sh
sudo zpool create -f -o ashift=12 -m <mount> <pool> [raidz(2|3)|mirror] <ids>
```

- `create`: subcommand to **create** the pool.
- `-f`: **F**orce creating the pool.  
This is to overcome the "EFI label error". See [#Does not contain an EFI label](https://wiki.archlinux.org/title/ZFS#Does_not_contain_an_EFI_label).
- `-o ashift=12`: Because correct detection of 4k disks is not reliable, `-o ashift=12` should always be specified during pool creation.
- `-m`: The **m**ount point of the pool.  
If this is not specified, then the pool will be mounted to `/<pool>`.
- `pool`: This is the name of the **pool**.
- `raidz(2|3)|mirror`: This is the type of virtual device (`vdev`) that will be created from the list of devices.  
    - Raidz (ZFS RAID) modes:  
    `raidz` is a single disk of parity (similar to RAID5);  
    `raidz2` for 2 disks of parity (RAID6);  
    `raidz3` for 3 disks of parity.  
    - `mirror` (RAID1 or RAID10) accepts a list of 2 to $n$ devices.  
    - RAID0 is achieved by not specifying the `vdev` type (each device will be added as one).  
    After creation, a device can be added to each single drive `vdev` to turn it into a `mirror`, which can be useful for migrating data.
- `ids`: The list of devices (drives or partitions).

> [!Tip]
> `export` your specific `<ids>` to environment variables to shorten `zpool` commands.
> 
> ```sh
> export DISK1="/dev/disk/by-id/nvme-Samsung_SSD_990_PRO_1TB_P2GKFC8E811407S"
> export DISK2="/dev/disk/by-id/nvme-Samsung_SSD_990_PRO_1TB_P2GKFC8E811580F"
> export DISK3="/dev/disk/by-id/nvme-Samsung_SSD_990_PRO_1TB_P2GKFC8E811584D"
> export DISK4="/dev/disk/by-id/nvme-Samsung_SSD_990_PRO_1TB_P2GKFC8E811671K"
> ```

For example, this stripe:

```sh
sudo zpool create -f -m /mnt/data bigdata \
  /dev/disk/by-id/nvme-Samsung_SSD_990_PRO_1TB_P2GKFC8E811407S \
  /dev/disk/by-id/nvme-Samsung_SSD_990_PRO_1TB_P2GKFC8E811580F
```

…becomes:

```sh
sudo zpool create -f -m /mnt/data bigdata $DISK1 $DISK2
```

Create pool with single raidz vdev:

```sh
sudo zpool create -f -m /mnt/data bigdata \
                  raidz \
                     $DISK1 \
                     $DISK2 \
                     $DISK3
```

Create pool with two mirror vdevs:

```sh
sudo zpool create -f -m /mnt/data bigdata \
                  mirror \
                     $DISK1 \
                     $DISK2 \
                  mirror \
                     $DISK3 \
                     $DISK4
```

Note that the 3 configurations above yield the same usable space (here, 2 TB).


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

Use a timer utility (`systemd`, `cron`, …) to `scrub` automatically.





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

1. Import the pool.  
Take care to use `-d /dev/disk/by-<whatever>` otherwise the pool might not auto-import on boot.


    ```sh
    sudo zpool import -d /dev/disk/by-id mypool
    ```

1. Check that it's properly mounted. Otherwise run `sudo zfs mount -a` .

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








