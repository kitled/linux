# `mdadm`
> ***M**ultiple **D**evices **Adm**inistration*

`mdadm` *(**m**ultiple **d**evices **adm**inistration) lets us 'merge' or 'group' several storage devices in RAID (redundant **array** of inexpensive/independent disks).  
An array is then seen, formatted, and used as one single \[ virtual \] device for increased capacity, reliability, and/or performance.*

📘`man`: [`mdadm`][man-mdadm]

Canonical command: `mdadm [mode] <raiddevice> [options] <component-devices>`





## Procedures

### Installation

```bash
sudo apt update
sudo apt install mdadm
```

### Create array

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


[man-mdadm]: https://manpages.ubuntu.com/manpages/noble/en/man8/mdadm.8.html