# Filesystems





## Choosing a filesystem

> [!Tip]
> **TL;DR**
>
> My recommendations on Linux are:
> - [Btrfs](Btrfs.md) for OS drives (single drive, or mirrors/RAID1)
> - [ZFS](ZFS.md) for everything else
>     - [Root on ZFS](ZFS/README.md#root-on-zfs) for the brave!
 
[**ZFS**](ZFS.md) is technically the best filesystem ever created in my opinion. It benefits a lot from a few fast drives for write/read caching, but above all ***as much RAM as you can give it***. [Learn ZFS](ZFS/README.md#learn-zfs) to understand why.

[**XFS**](XFS.md) (or even Ext4) is fine for most cases.

For large arrays that benefit from redundancy, use ZFS.

For OS drives, [**Btrfs**](Btrfs.md) shines on Linux (but **NEVER** in raid 5 or 6). It has nice rollback features. A Btrfs mirror is as safe and easy as it gets for Linux OS partitions.

