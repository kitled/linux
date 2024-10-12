# Storage

*Basic procedures to manage filesystems relevant to GHost (XFS, Btrfs, and ZFS) in a number of ways including software RAID.*

## Overview

> [!Tip]
> We manage a hypothetical  
> - storage unit called `data`  
> - located at `/mnt/data`  
> - composed of physical drives aliased `$disk1`, `$disk2`, …, `$diskN`

### Table of contents

`.` **Storage**  
`├──` [**`mount`**](mount.md)  
`├──` [**`mdadm`**](mdadm.md)  
`├──` [**Btrfs**](Btrfs.md)  
`├──` [**XFS**](XFS.md)  
`└──` [**ZFS**](ZFS.md)  


## What you need to know

Generally:

1. \[ *Optionally* \] **Multiple drives** may be grouped in an **array**.
    - This can be achieved at the hardware or software level (e.g., using [**`mdadm`**](mdadm.md)).  
        - **Software RAID** is usually what you want (it's portable, easier, and robust).
    - You must **do it first**, before formatting.
1. A drive or array must be **formatted** using a [filesystem](#choosing-a-filesystem).
1. A formatted filesystem must finally be [mounted](#mount) to **read/write** it.

For [ZFS](#zfs) and [Btrfs](#btrfs), these principles stand, but head over to their dedicated section as they manage things their own way.



Create the `mdadm` array first, except for ZFS and Btrfs which feature built-in device management.



## Choosing a filesystem

See notes in this directory for detailed procedures (links below, inline).

[**ZFS**](ZFS.md) is technically the best filesystem ever created in my opinion, but it's quite involved to setup well manually, and requires quite a bit of RAM.

[**XFS**](XFS.md) (or even Ext4) is fine for most cases.

If you have special needs:
- for large arrays that benefit from redundancy, consider (RAIDZ is awesomely robust; RAID1/mirror is insanely powerful; RAID0/stripe mode isn't particularly noteworthy).
- for OS drives, [**Btrfs**](Btrfs.md) shines on Linux (but **NEVER** in raid 5 or 6). It has nice rollback features. A Btrfs mirror is as safe and easy as it gets for Linux OS partitions.

*Note that [Bcachefs](https://bcachefs.org/) looks promising but is too new in my opinion.*






[man-mount]: https://manpages.ubuntu.com/manpages/noble/en/man8/mount.8.html
[man-xfs]: https://manpages.ubuntu.com/manpages/noble/en/man5/xfs.5.html
[man-mkfs.xfs]: https://manpages.ubuntu.com/manpages/noble/en/man8/mkfs.xfs.8.html
[man-chattr]: https://manpages.ubuntu.com/manpages/noble/en/man1/chattr.1.html
[man-mdadm]: https://manpages.ubuntu.com/manpages/noble/en/man8/mdadm.8.html


<!--
[man-]: 
-->














