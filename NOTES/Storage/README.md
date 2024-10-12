# Storage




## Overview

Manage physical drives and virtual storage spaces: format, mount, maintain.



### Table of contents

`.` **Storage**  
`├──` [**`mount`**](mount.md)  
`├──` [**`mdadm`**](mdadm.md)  
`├──` [**Btrfs**](Btrfs.md)  
`├──` [**XFS**](XFS.md)  
`└──` [**ZFS**](ZFS.md)  




### What you need to know

> [!Tip]
> **TL;DR**
> - if multiple drives, create the [`mdadm`](mdadm.md) array first.  
(Except for [ZFS](ZFS.md) and [Btrfs](Btrfs.md) which feature built-in device management)  
>
>- In all cases, [format](#choosing-a-filesystem), for instance in [XFS](XFS.md), and finally [mount](mount.md) the partition.

Generally:

1. \[ *Optional* \] **Multiple drives** may be grouped in an **array**.
    - You must **do it first**, before formatting.
    - This can be achieved at the hardware or software level.
    - **Software RAID** (e.g., using [**`mdadm`**](mdadm.md)) is usually what you want.
        - Easier to manage (notably, no need to reboot to some BIOS tool).
        - Guaranteed portability between machines.
            - You just replug all drives and remake the same setup (install packages & copy config files).
        - Very robust as it integrates tightly with the OS.
1. A drive or array must be **formatted** using a [filesystem](#choosing-a-filesystem). If in doubt, use [XFS](XFS.md).
1. A formatted filesystem must finally be [mounted](mount.md) to **read/write** it.
    - Manually in your shell.
    - As a line in the `/etc/fstab` file to auto-mount at boot time.


> [!Note]
> In these notes, we manage a hypothetical  
> - storage unit called `data`  
> - located at `/mnt/data`  
> - composed of physical drives aliased `$disk1`, `$disk2`, …, `$diskN`






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














