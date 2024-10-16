# `zpool`

ZFS pool management




## Overview

⏩ `Handbook` https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zpool

> [!Important]
> If you already have pools setup in prior installations, skip to Import.


## `create`






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

    ⮟ `stdout`

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

    ⮟ `stdout`

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
    # zpool status
    ```
    ⮟ `stdout`
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
    # zpool upgrade
    ```
    ⮟ `stdout`
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
    # zpool upgrade mypool
    ```
    ⮟ `stdout`
    ```
    This system supports ZFS pool feature flags.

    Successfully upgraded 'mypool' from version 28 to feature flags.
    Enabled the following features on 'mypool':
      async_destroy
      empty_bpobj
      lz4_compress
      multi_vdev_crash_dump
    ```






