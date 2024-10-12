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
`├──` [**Btrfs**](Btrfs.md)  
`├──` [**XFS**](XFS.md)  
`└──` [**ZFS**](ZFS.md)  

### In this file

- Arrays (`mdadm`)
- Filesystems
- Mount (`mount`)

## What you need to know

Generally:

- \[optional\] **Multiple drives** must first be grouped in an [array](#arrays).
    - This can be achieved at the hardware or software level (e.g., using `mdadm`).  
    - Software RAID is usually what you want (it's portable, easier, and robust).
- A drive or array must be **formatted** using a [filesystem](#filesystem).
- A formatted filesystem must finally be [mounted](#mount) to **read/write** it.

For [ZFS](#zfs) and [Btrfs](#btrfs), these principles stand, but head over to their dedicated section as they manage things their own way.




















## Arrays

Create the `mdadm` array first, except for ZFS and Btrfs which feature built-in device management.



### `mdadm`

`mdadm` *(**m**ultiple **d**evices **adm**inistration) lets us "merge" storage devices in RAID arrays, which are then seen (and formatted) as one virtual device (for increased capacity, reliability, and/or performance).*

📘`man`: [`mdadm`][man-mdadm]

Canonical command: `mdadm [mode] <raiddevice> [options] <component-devices>`



#### Installation

```bash
sudo apt update
sudo apt install mdadm
```

#### Create array

The **create** mode (`-C`|`--create`) will make:

- a Linux software **RAID device** (virtual device `/dev/md0`)
- over **`n` component devices** (`n` physical drives)
- set to RAID **level `l`** (`l`=`0`,`1`,`10`,`5`,`6`)

Example RAID level `0` over n=`3` devices:

```bash
sudo mdadm -Cv /dev/md0 -l0 -n3 \
/dev/disk/by-id/nvme-Samsung_SSD*{497K,512J,528K}
```

- `-v` = `--verbose`
- `-l0` = `--level=0` options are:  
`linear`,  
`raid0` | `0` | `stripe`,  
`raid1` | `1` | `mirror`,  
`raid4` | `4`,  
`raid5` | `5`,  
`raid6` | `6`,  
`raid10` | `10`,  
`multipath` | `mp`,  
`faulty`,  
`container`.  
Obviously, some of these are synonymous.

Check the creation of the RAID array:

```bash
cat /proc/mdstat
```

And further inspect the RAID:

```bash
sudo mdadm --detail /dev/md0
```


> [!Warning]
> After reboot, the `mdadm` RAID virtual device MAY have a different name.
>
> ```
> /dev/md127 on /mnt/data type xfs (rw,noatime,attr2,inode64,logbufs=8,logbsize=256k,sunit=1024,swidth=3072,noquota)
> ```
>
> Here `md127` instead of `md0`. Same `udev` behavior observed with `sda` or `nvme0` not being reliable names across boots; hence why we MUST use the `UUID` or some custom `LABEL` in deterministic scripts or configs like `fstab` or filesystem tools.
>
> Generally, if you only have the one of a couple `md` devices, using `/dev/md*` will do the trick and work across any minor name change (these are predictable to some extent).






## Filesystem

See notes in this directory for detailed procedures (links below, inline).

[**ZFS**](ZFS.md) is technically the best filesystem ever created in my opinion, but it's quite involved to setup well manually, and requires quite a bit of RAM.

[**XFS**](XFS.md) (or even Ext4) is fine for most cases.

If you have special needs:
- for large arrays that benefit from redundancy, consider (RAIDZ is awesomely robust; RAID1/mirror is insanely powerful; RAID0/stripe mode isn't particularly noteworthy).
- for OS drives, [**Btrfs**](Btrfs.md) shines on Linux (but **NEVER** in raid 5 or 6). It has nice rollback features. A Btrfs mirror is as safe and easy as it gets for Linux OS partitions.

*Note that [Bcachefs](https://bcachefs.org/) looks promising but is too new in my opinion.*





## Mount

📘`man`: [`mount`][man-mount]

Canonical form: `mount [-fnrsvw] [-t fstype] [-o options] device mountpoint`

To display the list of all currently mounted filesystems:

```
mount    # you generally want to `| grep something` here
```

### Basic use

A formatted device can be mounted to a directory using `mount` as superuser.

```bash
sudo mount -vm /dev/md0 /mnt/data
```

> [!Tip]
> `-m` (`--mkdir`) creates the mount point directory if it does not exist yet.

### `fstab` (boot mount)

Get the UUID of the device:

```bash
sudo blkid | grep /dev/<your-device>
```

Edit `/etc/fstab` to add an entry for the RAID 0 array:

```bash
sudo nano /etc/fstab
```

Add this line (replace `UUID` with the actual UUID from the `blkid` command):

```fstab
UUID=<your-uuid> /mnt/data xfs defaults,noatime 0 0
```

You can mount all `fstab` entries with `mount -a`.

```
sudo mount -a
```








[man-mount]: https://manpages.ubuntu.com/manpages/noble/en/man8/mount.8.html
[man-xfs]: https://manpages.ubuntu.com/manpages/noble/en/man5/xfs.5.html
[man-mkfs.xfs]: https://manpages.ubuntu.com/manpages/noble/en/man8/mkfs.xfs.8.html
[man-chattr]: https://manpages.ubuntu.com/manpages/noble/en/man1/chattr.1.html
[man-mdadm]: https://manpages.ubuntu.com/manpages/noble/en/man8/mdadm.8.html


<!--
[man-]: 
-->














