# Debian

https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/


## Installation

https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/index.html#installation

If you want to use ZFS as your root filesystem, see the [Root on ZFS](#root-on-zfs) section below instead.

ZFS packages are included in the [contrib repository](https://packages.debian.org/source/zfs-linux). The [backports repository](https://backports.debian.org/Instructions/) often provides newer releases of ZFS. You can use it as follows.

Add the backports repository:

```
vi /etc/apt/sources.list.d/bookworm-backports.list
```

```
deb http://deb.debian.org/debian bookworm-backports main contrib
deb-src http://deb.debian.org/debian bookworm-backports main contrib
```

```
vi /etc/apt/preferences.d/90_zfs
```

```
Package: src:zfs-linux
Pin: release n=bookworm-backports
Pin-Priority: 990
```

Install the packages:

```sh
apt update
apt install dpkg-dev linux-headers-generic linux-image-generic
apt install zfs-dkms zfsutils-linux
```


> [!Caution]
> If you are in a poorly configured environment (e.g. certain VM or container consoles), when apt attempts to pop up a message on first install, it may fail to notice a real console is unavailable, and instead appear to hang indefinitely. To circumvent this, you can prefix the apt install commands with `DEBIAN_FRONTEND=noninteractive`, like this:
>
> ```
> DEBIAN_FRONTEND=noninteractive apt install zfs-dkms zfsutils-linux
> ```










## Root on ZFS

https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/Debian%20Bookworm%20Root%20on%20ZFS.html

