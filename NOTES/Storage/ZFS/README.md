# ZFS
`•`  
`├──` [**`zpool`**](zpool.md)  
`├──` [**`zfs`**](zfs.md)  
`├──` [**Debian**](debian.md) install  
`└──` [**Ubuntu**](ubuntu.md) install  

> [!Tip]
> Quick Start:
>
> 1. Install ZFS.
> 1. Create a pool (array of drives).
> 1. Create a dataset with mountpoint.
>
> Done.

## Overview

> 🏛️ [`Home`][home] [**openzfs.org**][home]  
> 🧬 [`Repo`][repo] [github.com/**openzfs/zfs**][repo]  
> 📚 [`Docs`][docs] [**openzfs**.github.io/**openzfs-docs**][docs]  
> 📑 [`Wiki`][wiki] [openzfs.org/**wiki**][wiki]  
>
> ⏩ [`Handbook`][hand][^handbook]  
> 🅰️ [`Arch`][arch]  




### Features

OpenZFS is an open-source storage platform. It includes the functionality of both traditional file systems and volume manager. It has many advanced features including:

- Protection against data corruption. Integrity checking for both data and metadata.
- Continuous integrity verification and automatic “self-healing” repair
- Data redundancy with mirroring, RAID-Z1/2/3 [and DRAID]
- Support for high storage capacities — up to 256 trillion yobibytes ($2^{128}$ bytes)
- Space-saving with transparent compression using LZ4, GZIP or ZSTD
- Hardware-accelerated native encryption
- Efficient storage with snapshots and copy-on-write clones
- Efficient local or remote replication — send only changed blocks with ZFS send and receive

### Administration

ZFS administration uses two main utilities. 
- The [`zpool`](zpool.md) utility controls the operation of the pool and allows adding, removing, replacing, and managing disks. 
- The [`zfs`](zfs.md) utility allows creating, destroying, and managing datasets, both [file systems](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-term-filesystem) and [volumes](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-term-volume).



### Getting started

🔗 `Docs`☛ [Getting Started][getting-started]

> To get started with OpenZFS refer to the provided documentation for your distribution. It will cover the recommended installation method and any distribution specific information. First time OpenZFS users are encouraged to check out Aaron Toponce’s [excellent documentation](https://pthree.org/2012/04/17/install-zfs-on-debian-gnulinux/).

- OpenZFS manual pages  
`Docs`☛ [man][man]
- Feature Details. Detailed subsystem/feature blogs, on-disk format specifications  
`Wiki`☛ [Developer Resources][dev]



#### Debian

🔗 Installation: `Docs`☛ Getting Started / [Debian][deb]

🔗 Root on ZFS: `Docs`☛ Getting Started / Debian / [Debian Bookworm Root on ZFS][deb12-zfs-root]

[deb]: https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/
[deb12-zfs-root]: https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/Debian%20Bookworm%20Root%20on%20ZFS.html



#### Ubuntu

🔗 Installation: `Docs`☛ Getting Started/[Ubuntu][ubu]

[ubu]: https://openzfs.github.io/openzfs-docs/Getting%20Started/Ubuntu/



### Learn ZFS

Official project recommendations

- FAQ: https://openzfs.github.io/openzfs-docs/Project%20and%20Community/FAQ.html
- OpenZFS concepts: https://openzfs.org/wiki/Newcomers

Tom Lawrence's excellent explanations

- 
- 
- 
- 
- 





## ZoL (ZFS on Linux)

🔗 [**zfsonlinux.org**][zol]

ZFS on Linux is the official port of OpenZFS to Linux (ZFS was born on Solaris and lives on FreeBSD first and foremost afaik). It used to be somewhat of a kernel pain to deal with ZFS versions on Linux, and to some extent it's still not supported in the kernel itself, hence rarely shows up as an option for root filesystem (Ubuntu 'vanilla' with Gnome stands out as a notable exception).








[^handbook]: Written for FreeBSD, but ZFS commands (`zpool`, `zfs`) are the same.

<!-- LINKS -->
[home]: https://openzfs.org/
[repo]: https://github.com/openzfs/zfs/
[docs]: https://openzfs.github.io/openzfs-docs/
[wiki]: https://openzfs.org/wiki/
[hand]: https://docs.freebsd.org/en/books/handbook/zfs/ "Quick Start"
[arch]: https://wiki.archlinux.org/title/ZFS
[zol]: https://zfsonlinux.org/
[getting-started]: https://openzfs.github.io/openzfs-docs/Getting%20Started/
[man]: https://openzfs.github.io/openzfs-docs/man/
[dev]: https://openzfs.org/wiki/Developer_resources


