# Debian

https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/





## Installation

https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/index.html#installation

> [!Note]
> If you want to use ZFS as your root filesystem, see the [Root on ZFS](#root-on-zfs) section below instead.

ZFS packages are included in the [contrib repository](https://packages.debian.org/source/zfs-linux). The [backports repository](https://backports.debian.org/Instructions/) often provides newer releases of ZFS. You can use it as follows.

1. To add the backports repository, open the apt source file.

    ```
    sudo nano /etc/apt/sources.list.d/bookworm-backports.list
    ```

1. Write:

    ```
    deb http://deb.debian.org/debian bookworm-backports main contrib
    deb-src http://deb.debian.org/debian bookworm-backports main contrib
    ```

1. Create an `apt` preference file for ZFS.

    ```
    sudo nano /etc/apt/preferences.d/90_zfs
    ```

1. Write:

    ```
    Package: src:zfs-linux
    Pin: release n=bookworm-backports
    Pin-Priority: 990
    ```

1. Install generic Linux headers and image for your system.[^out1]

    ```sh
    sudo apt update
    sudo apt install dpkg-dev linux-headers-generic linux-image-generic
    ```

1. Install the ZFS packages.[^out2]

    ```sh
    sudo apt install zfs-dkms zfsutils-linux 
    ```

    The build step takes some time.  
    `Building initial module for 6.1.0-26-amd64`

    If all goes well, it ends by creating a bunch of symlinks and running `update-initramfs`.

> [!Caution]
> If you are in a poorly configured environment (e.g. certain VM or container consoles), when `apt` attempts to pop up a message on first install, it may fail to notice a real console is unavailable, and instead appear to hang indefinitely. To circumvent this, you can prefix the apt install commands with `DEBIAN_FRONTEND=noninteractive`, like this:
>
> ```
> DEBIAN_FRONTEND=noninteractive apt install zfs-dkms zfsutils-linux
> ```





## Root on ZFS

https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/Debian%20Bookworm%20Root%20on%20ZFS.html










---

**Footnotes**

[^out1]: 🖥️`stdout` for `sudo apt install dpkg-dev linux-headers-generic linux-image-generic`  
    ⮟

    ```
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    Note, selecting 'linux-headers-amd64' instead of 'linux-headers-generic'
    Note, selecting 'linux-image-amd64' instead of 'linux-image-generic'
    linux-image-amd64 is already the newest version (6.1.112-1).
    The following additional packages will be installed:
      binutils binutils-common binutils-x86-64-linux-gnu build-essential 
      cpp cpp-12 dirmngr fakeroot fontconfig-config fonts-dejavu-core 
      g++ g++-12 gcc gcc-12 gnupg gnupg-l10n gnupg-utils gpg gpg-agent 
      gpg-wks-client gpg-wks-server gpgconf gpgsm libabsl20220623 
      libalgorithm-diff-perl libalgorithm-diff-xs-perl libalgorithm-merge-perl 
      libaom3 libasan8 libassuan0 libatomic1 libavif15 libbinutils libc-dev-bin 
      libc-devtools libc6-dev libcc1-0 libcrypt-dev libctf-nobfd0 libctf0 
      libdav1d6 libde265-0 libdeflate0 libdpkg-perl libfakeroot 
      libfile-fcntllock-perl libfontconfig1 libgav1-1 libgcc-12-dev libgd3 
      libgomp1 libgprofng0 libheif1 libisl23 libitm1 libjbig0 libksba8 liblerc4 
      liblsan0 libmpc3 libmpfr6 libnpth0 libnsl-dev libnuma1 libquadmath0 
      librav1e0 libstdc++-12-dev libsvtav1enc1 libtiff6 libtirpc-dev libtsan2 
      libubsan1 libwebp7 libx265-199 libxpm4 libyuv0 linux-compiler-gcc-12-x86 
      linux-headers-6.1.0-26-amd64 linux-headers-6.1.0-26-common linux-kbuild-6.1 
      linux-libc-dev make manpages-dev pinentry-curses rpcsvc-proto
    Suggested packages:
      binutils-doc cpp-doc gcc-12-locales cpp-12-doc pinentry-gnome3 tor 
      debian-keyring g++-multilib g++-12-multilib gcc-12-doc gcc-multilib 
      autoconf automake libtool flex bison gdb gcc-doc gcc-12-multilib parcimonie 
      xloadimage scdaemon glibc-doc bzr libgd-tools libstdc++-12-doc make-doc 
      pinentry-doc
    The following NEW packages will be installed:
      binutils binutils-common binutils-x86-64-linux-gnu build-essential 
      cpp cpp-12 dirmngr dpkg-dev fakeroot fontconfig-config fonts-dejavu-core 
      g++ g++-12 gcc gcc-12 gnupg gnupg-l10n gnupg-utils gpg gpg-agent 
      gpg-wks-client gpg-wks-server gpgconf gpgsm libabsl20220623 
      libalgorithm-diff-perl libalgorithm-diff-xs-perl libalgorithm-merge-perl 
      libaom3 libasan8 libassuan0 libatomic1 libavif15 libbinutils libc-dev-bin 
      libc-devtools libc6-dev libcc1-0 libcrypt-dev libctf-nobfd0 libctf0 libdav1d6 
      libde265-0 libdeflate0 libdpkg-perl libfakeroot libfile-fcntllock-perl 
      libfontconfig1 libgav1-1 libgcc-12-dev libgd3 libgomp1 libgprofng0 libheif1 
      libisl23 libitm1 libjbig0 libksba8 liblerc4 liblsan0 libmpc3 libmpfr6 
      libnpth0 libnsl-dev libnuma1 libquadmath0 librav1e0 libstdc++-12-dev 
      libsvtav1enc1 libtiff6 libtirpc-dev libtsan2 libubsan1 libwebp7 libx265-199 
      libxpm4 libyuv0 linux-compiler-gcc-12-x86 linux-headers-6.1.0-26-amd64 
      linux-headers-6.1.0-26-common linux-headers-amd64 linux-kbuild-6.1 
      linux-libc-dev make manpages-dev pinentry-curses rpcsvc-proto
    0 upgraded, 87 newly installed, 0 to remove and 0 not upgraded.
    Need to get 99.8 MB of archives.
    After this operation, 394 MB of additional disk space will be used.
    Do you want to continue? [Y/n]
    ```

[^out2]: 🖥️`stdout` for `sudo apt install zfs-dkms zfsutils-linux`  
    ⮟

    ```
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    The following additional packages will be installed:
      dkms libnvpair3linux libuutil3linux libzfs4linux libzpool5linux zfs-zed
    Suggested packages:
      menu debhelper nfs-kernel-server samba-common-bin zfs-initramfs | zfs-dracut
    The following NEW packages will be installed:
      dkms libnvpair3linux libuutil3linux libzfs4linux libzpool5linux zfs-dkms 
      zfs-zed zfsutils-linux
    0 upgraded, 8 newly installed, 0 to remove and 0 not upgraded.
    Need to get 4,788 kB of archives.
    After this operation, 26.9 MB of additional disk space will be used.
    Do you want to continue? [Y/n] 
    ```

    ⮟

    ```
     ┌─────────────────────────────────────────────────────────────────────────────────────┤ Configuring zfs-dkms ├──────────────────────────────────────────────────────────────────────────────────────┐
     │                                                                                                                                                                                                   │ 
     │ Licenses of OpenZFS and Linux are incompatible                                                                                                                                                    │ 
     │                                                                                                                                                                                                   │ 
     │ OpenZFS is licensed under the Common Development and Distribution License (CDDL), and the Linux kernel is licensed under the GNU General Public License Version 2 (GPL-2). While both are free    │ 
     │ open source licenses they are restrictive licenses. The combination of them causes problems because it prevents using pieces of code exclusively available under one license with pieces of code  │ 
     │ exclusively available under the other in the same binary.                                                                                                                                         │ 
     │                                                                                                                                                                                                   │ 
     │ You are going to build OpenZFS using DKMS in such a way that they are not going to be built into one monolithic binary. Please be aware that distributing both of the binaries in the same media  │ 
     │ (disk images, virtual appliances, etc) may lead to infringing.                                                                                                                                    │ 
     │                                                                                                                                                                                                   │ 
     │                                                                                              <Ok>                                                                                                 │ 
     │                                                                                                                                                                                                   │ 
     └───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘ 
    ```

    ⮟

    ```
    Get:1 http://deb.debian.org/debian bookworm/main amd64 dkms all 3.0.10-8+deb12u1 [48.7 kB]
    Get:2 http://deb.debian.org/debian bookworm-backports/contrib amd64 zfs-dkms all 2.2.6-1~bpo12+3 [2,416 kB]
    Get:3 http://deb.debian.org/debian bookworm-backports/contrib amd64 libnvpair3linux amd64 2.2.6-1~bpo12+3 [61.0 kB]
    Get:4 http://deb.debian.org/debian bookworm-backports/contrib amd64 libuutil3linux amd64 2.2.6-1~bpo12+3 [52.1 kB]
    Get:5 http://deb.debian.org/debian bookworm-backports/contrib amd64 libzfs4linux amd64 2.2.6-1~bpo12+3 [229 kB]
    Get:6 http://deb.debian.org/debian bookworm-backports/contrib amd64 libzpool5linux amd64 2.2.6-1~bpo12+3 [1,339 kB]
    Get:7 http://deb.debian.org/debian bookworm-backports/contrib amd64 zfsutils-linux amd64 2.2.6-1~bpo12+3 [563 kB]
    Get:8 http://deb.debian.org/debian bookworm-backports/contrib amd64 zfs-zed amd64 2.2.6-1~bpo12+3 [80.1 kB]
    Fetched 4,788 kB in 0s (21.0 MB/s)
    Preconfiguring packages ...
    Selecting previously unselected package dkms.
    (Reading database ... 60409 files and directories currently installed.)
    Preparing to unpack .../0-dkms_3.0.10-8+deb12u1_all.deb ...
    Unpacking dkms (3.0.10-8+deb12u1) ...
    Selecting previously unselected package zfs-dkms.
    Preparing to unpack .../1-zfs-dkms_2.2.6-1~bpo12+3_all.deb ...
    Unpacking zfs-dkms (2.2.6-1~bpo12+3) ...
    Selecting previously unselected package libnvpair3linux.
    Preparing to unpack .../2-libnvpair3linux_2.2.6-1~bpo12+3_amd64.deb ...
    Unpacking libnvpair3linux (2.2.6-1~bpo12+3) ...
    Selecting previously unselected package libuutil3linux.
    Preparing to unpack .../3-libuutil3linux_2.2.6-1~bpo12+3_amd64.deb ...
    Unpacking libuutil3linux (2.2.6-1~bpo12+3) ...
    Selecting previously unselected package libzfs4linux.
    Preparing to unpack .../4-libzfs4linux_2.2.6-1~bpo12+3_amd64.deb ...
    Unpacking libzfs4linux (2.2.6-1~bpo12+3) ...
    Selecting previously unselected package libzpool5linux.
    Preparing to unpack .../5-libzpool5linux_2.2.6-1~bpo12+3_amd64.deb ...
    Unpacking libzpool5linux (2.2.6-1~bpo12+3) ...
    Selecting previously unselected package zfsutils-linux.
    Preparing to unpack .../6-zfsutils-linux_2.2.6-1~bpo12+3_amd64.deb ...
    Unpacking zfsutils-linux (2.2.6-1~bpo12+3) ...
    Selecting previously unselected package zfs-zed.
    Preparing to unpack .../7-zfs-zed_2.2.6-1~bpo12+3_amd64.deb ...
    Unpacking zfs-zed (2.2.6-1~bpo12+3) ...
    Setting up libnvpair3linux (2.2.6-1~bpo12+3) ...
    Setting up dkms (3.0.10-8+deb12u1) ...
    Setting up zfs-dkms (2.2.6-1~bpo12+3) ...
    Loading new zfs-2.2.6 DKMS files...
    Building for 6.1.0-26-amd64
    Building initial module for 6.1.0-26-amd64
    Done.
    
    zfs.ko:
    Running module version sanity check.
     - Original module
       - No original module exists within this kernel
     - Installation
       - Installing to /lib/modules/6.1.0-26-amd64/updates/dkms/
    
    spl.ko:
    Running module version sanity check.
     - Original module
       - No original module exists within this kernel
     - Installation
       - Installing to /lib/modules/6.1.0-26-amd64/updates/dkms/
    depmod...
    Setting up libuutil3linux (2.2.6-1~bpo12+3) ...
    Setting up libzpool5linux (2.2.6-1~bpo12+3) ...
    Setting up libzfs4linux (2.2.6-1~bpo12+3) ...
    Setting up zfsutils-linux (2.2.6-1~bpo12+3) ...
    insmod /lib/modules/6.1.0-26-amd64/updates/dkms/spl.ko 
    insmod /lib/modules/6.1.0-26-amd64/updates/dkms/zfs.ko 
    Created symlink /etc/systemd/system/zfs-import.target.wants/zfs-import-cache.service → /lib/systemd/system/zfs-import-cache.service.
    Created symlink /etc/systemd/system/zfs.target.wants/zfs-import.target → /lib/systemd/system/zfs-import.target.
    Created symlink /etc/systemd/system/zfs-mount.service.wants/zfs-load-module.service → /lib/systemd/system/zfs-load-module.service.
    Created symlink /etc/systemd/system/zfs.target.wants/zfs-load-module.service → /lib/systemd/system/zfs-load-module.service.
    Created symlink /etc/systemd/system/zfs.target.wants/zfs-mount.service → /lib/systemd/system/zfs-mount.service.
    Created symlink /etc/systemd/system/zfs.target.wants/zfs-share.service → /lib/systemd/system/zfs-share.service.
    Created symlink /etc/systemd/system/zfs-volumes.target.wants/zfs-volume-wait.service → /lib/systemd/system/zfs-volume-wait.service.
    Created symlink /etc/systemd/system/zfs.target.wants/zfs-volumes.target → /lib/systemd/system/zfs-volumes.target.
    Created symlink /etc/systemd/system/multi-user.target.wants/zfs.target → /lib/systemd/system/zfs.target.
    zfs-import-scan.service is a disabled or a static unit, not starting it.
    Processing triggers for initramfs-tools (0.142+deb12u1) ...
    update-initramfs: Generating /boot/initrd.img-6.1.0-26-amd64
    Processing triggers for libc-bin (2.36-9+deb12u8) ...
    Processing triggers for man-db (2.11.2-2) ...
    Setting up zfs-zed (2.2.6-1~bpo12+3) ...
    Created symlink /etc/systemd/system/zed.service → /lib/systemd/system/zfs-zed.service.
    Created symlink /etc/systemd/system/zfs.target.wants/zfs-zed.service → /lib/systemd/system/zfs-zed.service.
    ```

[^]: 