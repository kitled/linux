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

1. Then install `zfsutils-linux`

```sh
apt update
apt install zfsutils-linux
```




## Root on ZFS

https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/Debian%20Bookworm%20Root%20on%20ZFS.html





## Footnotes

[^out1]: ⮟ `stdout` for `sudo apt install dpkg-dev linux-headers-generic linux-image-generic`  
    ```
    ...
    ```

