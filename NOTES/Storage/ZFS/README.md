# ZFS
`•`  
`├──` [**`zpool`**](zpool.md)  
`├──` [**`zfs`**](zfs.md)  
`└──` [Install](Install)  
`    ├──` [**Debian**](Install/Debian.md)  
`    └──` [**Ubuntu**](Install/Ubuntu.md)  
> [!Tip]
> Quick Start:
>
> 1. [Install](Install) ZFS.
> 1. Either:
>     - [`zpool import`](zpool.md#import) an existing pool. Done.
>     - [`zpool create`](zpool.md#create) a new one,
>         - [`zfs create`](zfs.md#datasets) a dataset. Done.
>
> ⚠️ Setup [`zpool scrub`](zpool.md#scrub) on a timer!
>
> See ⏩`Handbook`☛ [Quick Start Guide](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-quickstart-single-disk-pool) for a TL;DR of basic features.




## Overview

> 🏛️ [`Home`][home] (General) [**openzfs.org**][home]  
> 🐧 [`Home`][zol] (Linux) [**zfsonlinux.org**][zol]  
>
> Official  
> 🧬 [`Repo`][repo] [github.com/**openzfs/zfs**][repo]  
> 📚 [`Docs`][docs] [**openzfs**.github.io/**openzfs-docs**][docs]  
> 📑 [`Wiki`][wiki] [openzfs.org/**wiki**][wiki]  
>
> Third-party  
> ⏩ [`Handbook`][hand][^handbook])  
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



## Getting started

🔗 `Docs`☛ [Getting Started][getting-started]

> To get started with OpenZFS refer to the provided documentation for your distribution. It will cover the recommended installation method and any distribution specific information. First time OpenZFS users are encouraged to check out Aaron Toponce’s [excellent documentation](https://pthree.org/2012/04/17/install-zfs-on-debian-gnulinux/).

- OpenZFS manual pages  
`Docs`☛ [man][man]
- Feature Details. Detailed subsystem/feature blogs, on-disk format specifications  
`Wiki`☛ [Developer Resources][dev]



### Debian

🔗 Installation: `Docs`☛ Getting Started / [Debian][deb]

[deb]: https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/


### Ubuntu

🔗 Installation: `Docs`☛ Getting Started / [Ubuntu][ubu]

[ubu]: https://openzfs.github.io/openzfs-docs/Getting%20Started/Ubuntu/





## Root on ZFS

- Debian 12: `Docs`☛ Getting Started / Debian / [Debian **Bookworm** Root on ZFS][deb12-zfs-root]
- Ubuntu 22.04: `Docs`☛ Getting Started / Ubuntu / [Ubuntu **22.04** Root on ZFS][ubu22-zfs-root]


[deb12-zfs-root]: https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/Debian%20Bookworm%20Root%20on%20ZFS.html
[ubu22-zfs-root]: https://openzfs.github.io/openzfs-docs/Getting%20Started/Ubuntu/Ubuntu%2022.04%20Root%20on%20ZFS.html





## Learn ZFS

Official project recommendations

- FAQ: https://openzfs.github.io/openzfs-docs/Project%20and%20Community/FAQ.html
- OpenZFS concepts: https://openzfs.org/wiki/Newcomers

Tom Lawrence's excellent explanations (high-level, usually on TrueNAS Scale):

- 
- 
- 
- 
- 



> ⏩`Handbook`☛ [**What Makes ZFS Different**](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-differences)
>
> More than a file system, ZFS is fundamentally different from traditional file systems. Combining the traditionally separate roles of volume manager and file system provides ZFS with unique advantages. The file system is now aware of the underlying structure of the disks. Traditional file systems could exist on a single disk alone at a time. If there were two disks then creating two separate file systems was necessary. A traditional hardware RAID configuration avoided this problem by presenting the operating system with a single logical disk made up of the space provided by physical disks on top of which the operating system placed a file system. Even with software RAID solutions like those provided by GEOM, the UFS file system living on top of the RAID believes it’s dealing with a single device. ZFS' combination of the volume manager and the file system solves this and allows the creation of file systems that all share a pool of available storage. One big advantage of ZFS' awareness of the physical disk layout is that existing file systems grow automatically when adding extra disks to the pool. This new space then becomes available to the file systems. ZFS can also apply different properties to each file system. This makes it useful to create separate file systems and datasets instead of a single monolithic file system.




## Footnotes

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


