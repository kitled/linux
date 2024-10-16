# ZFS
 
> 🏛️ [`Home`][home] [**openzfs.org**][home]  
> 🧬 [`Repo`][repo] [github.com/**openzfs/zfs**][repo]  
> 📚 [`Docs`][docs] [**openzfs**.github.io/**openzfs-docs**][docs]  
> 📑 [`Wiki`][wiki] [openzfs.org/**wiki**][wiki]  



## Overview


OpenZFS is an open-source storage platform. It includes the functionality of both traditional file systems and volume manager. It has many advanced features including:

- Protection against data corruption. Integrity checking for both data and metadata.
- Continuous integrity verification and automatic “self-healing” repair
- Data redundancy with mirroring, RAID-Z1/2/3 [and DRAID]
- Support for high storage capacities — up to 256 trillion yobibytes ($2^{128}$ bytes)
- Space-saving with transparent compression using LZ4, GZIP or ZSTD
- Hardware-accelerated native encryption
- Efficient storage with snapshots and copy-on-write clones
- Efficient local or remote replication — send only changed blocks with ZFS send and receive



### Getting started

🔗 https://openzfs.github.io/openzfs-docs/Getting%20Started/

> To get started with OpenZFS refer to the provided documentation for your distribution. It will cover the recommended installation method and any distribution specific information. First time OpenZFS users are encouraged to check out Aaron Toponce’s [excellent documentation](https://pthree.org/2012/04/17/install-zfs-on-debian-gnulinux/).



- How to install OpenZFS: https://openzfs.github.io/openzfs-docs/Getting%20Started/
- OpenZFS manual pages: https://openzfs.github.io/openzfs-docs/man/
- Feature Details. Detailed subsystem/feature blogs, on-disk format specifications: [Developer Resources](https://openzfs.org/wiki/Developer_resources)


#### Debian

🔗 Installation: https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/

#### Ubuntu

🔗 Installation: https://openzfs.github.io/openzfs-docs/Getting%20Started/Ubuntu/



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











<!-- LINKS -->
[home]: https://openzfs.org/
[repo]: https://github.com/openzfs/zfs/
[docs]: https://openzfs.github.io/openzfs-docs/
[wiki]: https://openzfs.org/wiki/
[zol]: https://zfsonlinux.org/
