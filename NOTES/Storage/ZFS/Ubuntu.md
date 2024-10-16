# Ubuntu

https://openzfs.github.io/openzfs-docs/Getting%20Started/Ubuntu/





## Installation

https://openzfs.github.io/openzfs-docs/Getting%20Started/Ubuntu/#installation

> [!Note]
> If you want to use ZFS as your root filesystem, see the [Root on ZFS](#root-on-zfs) section below instead.

On Ubuntu, ZFS is included in the default Linux kernel packages. 

To install the ZFS utilities, first make sure `universe` is enabled.

1. Open `/etc/apt/sources.list`

    ```sh
    sudo nano /etc/apt/sources.list
    ```

1. Write:

    ```
    deb http://archive.ubuntu.com/ubuntu <CODENAME> main universe
    ```

1. Then install `zfsutils-linux`[^out1]

    ```sh
    sudo apt update
    sudo apt install zfsutils-linux
    ```




## Root on ZFS

https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/Debian%20Bookworm%20Root%20on%20ZFS.html





## Footnotes

[^out1]: `stdout` for `sudo apt install zfsutils-linux`  
    ⮟  

    ``` 
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    The following packages were automatically installed and are no longer required:
      linux-headers-6.8.0-44 linux-headers-6.8.0-44-generic linux-image-6.8.0-44-generic linux-modules-6.8.0-44-generic
      linux-modules-extra-6.8.0-44-generic linux-tools-6.8.0-44 linux-tools-6.8.0-44-generic
    Use 'sudo apt autoremove' to remove them.
    The following additional packages will be installed:
      libnvpair3linux libuutil3linux libzfs4linux libzpool5linux zfs-zed
    Suggested packages:
      nfs-kernel-server zfs-initramfs | zfs-dracut
    The following NEW packages will be installed:
      libnvpair3linux libuutil3linux libzfs4linux libzpool5linux zfs-zed zfsutils-linux
    0 upgraded, 6 newly installed, 0 to remove and 12 not upgraded.
    Need to get 2,356 kB of archives.
    After this operation, 7,393 kB of additional disk space will be used.
    Do you want to continue? [Y/n] 
    ```  
    ⮟  
    ```
    Get:1 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libnvpair3linux amd64 2.2.2-0ubuntu9.1 [61.6 kB]
    Get:2 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libuutil3linux amd64 2.2.2-0ubuntu9.1 [52.7 kB]
    Get:3 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libzfs4linux amd64 2.2.2-0ubuntu9.1 [226 kB]
    Get:4 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libzpool5linux amd64 2.2.2-0ubuntu9.1 [1,397 kB]
    Get:5 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 zfsutils-linux amd64 2.2.2-0ubuntu9.1 [551 kB]
    Get:6 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 zfs-zed amd64 2.2.2-0ubuntu9.1 [67.9 kB]
    Fetched 2,356 kB in 0s (8,336 kB/s)
    Selecting previously unselected package libnvpair3linux.
    (Reading database ... 277309 files and directories currently installed.)
    Preparing to unpack .../0-libnvpair3linux_2.2.2-0ubuntu9.1_amd64.deb ...
    Unpacking libnvpair3linux (2.2.2-0ubuntu9.1) ...
    Selecting previously unselected package libuutil3linux.
    Preparing to unpack .../1-libuutil3linux_2.2.2-0ubuntu9.1_amd64.deb ...
    Unpacking libuutil3linux (2.2.2-0ubuntu9.1) ...
    Selecting previously unselected package libzfs4linux.
    Preparing to unpack .../2-libzfs4linux_2.2.2-0ubuntu9.1_amd64.deb ...
    Unpacking libzfs4linux (2.2.2-0ubuntu9.1) ...
    Selecting previously unselected package libzpool5linux.
    Preparing to unpack .../3-libzpool5linux_2.2.2-0ubuntu9.1_amd64.deb ...
    Unpacking libzpool5linux (2.2.2-0ubuntu9.1) ...
    Selecting previously unselected package zfsutils-linux.
    Preparing to unpack .../4-zfsutils-linux_2.2.2-0ubuntu9.1_amd64.deb ...
    Unpacking zfsutils-linux (2.2.2-0ubuntu9.1) ...
    Selecting previously unselected package zfs-zed.
    Preparing to unpack .../5-zfs-zed_2.2.2-0ubuntu9.1_amd64.deb ...
    Unpacking zfs-zed (2.2.2-0ubuntu9.1) ...
    Setting up libnvpair3linux (2.2.2-0ubuntu9.1) ...
    Setting up libuutil3linux (2.2.2-0ubuntu9.1) ...
    Setting up libzpool5linux (2.2.2-0ubuntu9.1) ...
    Setting up libzfs4linux (2.2.2-0ubuntu9.1) ...
    Setting up zfsutils-linux (2.2.2-0ubuntu9.1) ...
    insmod /lib/modules/6.8.0-47-generic/kernel/zfs/spl.ko.zst 
    insmod /lib/modules/6.8.0-47-generic/kernel/zfs/zfs.ko.zst 
    Created symlink /etc/systemd/system/zfs-import.target.wants/zfs-import-cache.service → /usr/lib/systemd/system/zfs-import-cache.service.
    Created symlink /etc/systemd/system/zfs.target.wants/zfs-import.target → /usr/lib/systemd/system/zfs-import.target.
    Created symlink /etc/systemd/system/zfs-mount.service.wants/zfs-load-module.service → /usr/lib/systemd/system/zfs-load-module.service.
    Created symlink /etc/systemd/system/zfs.target.wants/zfs-load-module.service → /usr/lib/systemd/system/zfs-load-module.service.
    Created symlink /etc/systemd/system/zfs.target.wants/zfs-mount.service → /usr/lib/systemd/system/zfs-mount.service.
    Created symlink /etc/systemd/system/zfs.target.wants/zfs-share.service → /usr/lib/systemd/system/zfs-share.service.
    Created symlink /etc/systemd/system/zfs-volumes.target.wants/zfs-volume-wait.service → /usr/lib/systemd/system/zfs-volume-wait.service.
    Created symlink /etc/systemd/system/zfs.target.wants/zfs-volumes.target → /usr/lib/systemd/system/zfs-volumes.target.
    Created symlink /etc/systemd/system/multi-user.target.wants/zfs.target → /usr/lib/systemd/system/zfs.target.
    zfs-import-scan.service is a disabled or a static unit, not starting it.
    Setting up zfs-zed (2.2.2-0ubuntu9.1) ...
    Created symlink /etc/systemd/system/zed.service → /usr/lib/systemd/system/zfs-zed.service.
    Created symlink /etc/systemd/system/zfs.target.wants/zfs-zed.service → /usr/lib/systemd/system/zfs-zed.service.
    Processing triggers for man-db (2.12.0-4build2) ...
    Processing triggers for libc-bin (2.39-0ubuntu8.3) ...
    ```

