# Storage

Manage physical drives and virtual storage spaces: format, mount, permissions, maintain…

> [!Note]  
> For sharing, see [Net](../Net/) (Network) section.  
For instance, [NFS](../Net/NFS.md).


## Overview

> [!Tip]
> In this section, we manage a hypothetical, generic  
> - storage unit called `data`  
> - located at `/data`  
> - composed of physical devices aliased `$disk1`, `$disk2`, …`$diskN`.

### Table of contents

<!-- LINKS -->
[NS]: NS.md#persistent-block-device-naming
[Btrfs]: FS/Btrfs.md

`.` **Storage**  
`├──` [**NS**](NS.md) ⮜ NameSpaces  
`├──` [**FS**](FS)  ⮜ FileSystems  
`|   ├──` [**Btrfs**][Btrfs]  
`|   ├──` [**ZFS**](FS/ZFS)  
`|   └──` [**XFS**](FS/XFS.md)  
`└──` [**Tools**](Tools)  
`    ├──` [**`mount`**](Tools/mount.md)  
`    └──` [**`mdadm`**](Tools/mdadm.md)  




## What you need to know

> [!Tip]
> **TL;DR**
> - Always use **persistent device [names][NS]**, like hardware serial (in `/dev/disk/by-id`) or `by-partlabel`.
> 
> - If in doubt:
>     - **OS**: use [**Btrfs**](Btrfs.md) in single or mirror (RAID 1)
>     - **All else**: [**ZFS**](ZFS)
> 
> - If using [XFS](XFS.md) (or Ext4) with multiple drives:
>     - Create the [`mdadm`](mdadm.md) array first
>     - Then [format](XFS.md#example-setup) it.
>     - Finally [`mount`](mount.md) the partition
> - Purchase: **prefer more, smaller drives** for a given total (sum) capacity, assuming equivalent cost (if needed, you may use PCIe cards to add more SATA/NVMe ports to a machine).
>
> For instance, **4 ×** 1TB is **better** than a **single** 4TB drive.  
> More drives opens parallelization and redundancy options (RAID) which improve performance and reliability of storage.

Generally:

1. \[ *Optional* \] **Multiple drives** may be grouped in an **array**.
    - You must **do it first**, before formatting.
    - This can be achieved at the hardware or software level.
    - **Software RAID** (e.g., using `mdadm`) is usually what you want.
        - That's what ZFS and Btrfs do in their own device management.
        - Easier to manage (notably, no need to reboot to some BIOS tool).
        - Guaranteed portability between machines.
            - You just replug all drives,
            - and re-do the same setup.  
            (install packages, copy config files, or run commands).
        - Very robust as it integrates tightly with the OS.
1. A drive or array must be **formatted** using a [filesystem](#choosing-a-filesystem). ZFS and Btrfs do it automatically, XFS requires using `mkfs.xfs`.
1. A formatted filesystem must finally be [mounted](mount.md) to **read/write** it.
    - Manually in your shell (won't persist through reboot) using `mount`.
    - As a line in the `/etc/fstab` file to auto-mount at boot time.





## Choosing a filesystem

> [!Tip]
> **TL;DR**
>
> My recommendations on Linux are:
> - [Btrfs][Btrfs] for OS drive (single or ideally mirror)
> - [ZFS][ZFS] for everything else
>     - [Root on ZFS](FS/ZFS/Install/README.md#root-on-zfs) for the brave!

See [`FS`](FS) for details and procedures.






[man-mount]: https://manpages.ubuntu.com/manpages/noble/en/man8/mount.8.html
[man-xfs]: https://manpages.ubuntu.com/manpages/noble/en/man5/xfs.5.html
[man-mkfs.xfs]: https://manpages.ubuntu.com/manpages/noble/en/man8/mkfs.xfs.8.html
[man-chattr]: https://manpages.ubuntu.com/manpages/noble/en/man1/chattr.1.html
[man-mdadm]: https://manpages.ubuntu.com/manpages/noble/en/man8/mdadm.8.html


<!-- LINKS -->
[names]: NS.md#persistent-block-device-naming
[Btrfs]: FS/Btrfs.md

<!--
[man-]: 
-->














