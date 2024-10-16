# Debian

https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/





## Installation

https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/index.html#installation

If you want to use ZFS as your root filesystem, see the [Root on ZFS](#root-on-zfs) section below instead.

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

1. Install generic Linux headers and image for your system.  
    See [`stdout`][^out1]

    ```sh
    sudo apt update
    sudo apt install dpkg-dev linux-headers-generic linux-image-generic
    ```

    ```sh
    sudo apt install dpkg-dev linux-headers-generic linux-image-generic
    ```

1. Install the ZFS packages (see [`stdout`][^out2]).

    ```sh
    sudo apt install zfs-dkms zfsutils-linux 
    ```

> [!Caution]
> If you are in a poorly configured environment (e.g. certain VM or container consoles), when `apt` attempts to pop up a message on first install, it may fail to notice a real console is unavailable, and instead appear to hang indefinitely. To circumvent this, you can prefix the apt install commands with `DEBIAN_FRONTEND=noninteractive`, like this:
>
> ```
> DEBIAN_FRONTEND=noninteractive apt install zfs-dkms zfsutils-linux
> ```





## Root on ZFS

https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/Debian%20Bookworm%20Root%20on%20ZFS.html





# Footnotes

[^out1]: ⮟ `stdout` for `sudo apt install dpkg-dev linux-headers-generic linux-image-generic`  
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

[^out2]: ⮟ `stdout` for `sudo apt install zfs-dkms zfsutils-linux`  
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