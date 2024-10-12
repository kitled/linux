# XFS

📘`man`: [`xfs`][man-xfs], [`mkfs.xfs`][man-mkfs.xfs]



## HOW TO Procedures

### Create the XFS filesystem

Format the device with the XFS filesystem.  
*Here we use a RAID device `/dev/md0` previously created with [`mdadm`](#mdadm).*

```bash
sudo mkfs.xfs -L data /dev/md0
```



### XFS over `mdadm`

On modern Linux, leave it to defaults, unless you really know why.

> `xfsprogs` and the `mkfs.xfs` utility automatically select the best stripe size and stripe width for underlying devices that support it, such as Linux software RAID devices.
>
> …
>
> To check the parameters in use for an XFS filesystem, use xfs_info.
>
> ```bash
> xfs_info /dev/md0
> ```
>
> ```
> meta-data=/dev/md0               isize=256    agcount=32, agsize=45785440 blks
>          =                       sectsz=4096  attr=2
> data     =                       bsize=4096   blocks=1465133952, imaxpct=5
>          =                       sunit=16     swidth=48 blks
> naming   =version 2              bsize=4096   ascii-ci=0
> log      =internal               bsize=4096   blocks=521728, version=2
>          =                       sectsz=4096  sunit=1 blks, lazy-count=0
> realtime =none                   extsz=196608 blocks=0, rtextents=0
> ```
>
> 🔗 <https://raid.wiki.kernel.org/index.php/RAID_setup#XFS>


### `mount` options

Generally keep defaults.


##### `discard`|`nodiscard`

Note: It is currently recommended that you use the `fstrim` application to discard unused blocks rather than the `discard` mount option because the performance impact of this option is quite severe. For this reason, **`nodiscard`** is the **default**.


##### `logbsize=value`

Set the size of each in-memory log buffer.  
Valid sizes for version 2 logs also include `65536` (value=`64k`),`131072` (value=`128k`) and `262144` (value=`256k`).

The  default value for version 1 logs is `32768`, while the default value for version 2 logs is `max(32768, log_sunit)`.


##### Deprecated

Smell obsolete guides.

```
Name                        Removed
----                        -------
delaylog/nodelaylog         v4.0
ihashsize                   v4.0
irixsgid                    v4.0
osyncisdsync/osyncisosync   v4.0
barrier/nobarrier           v4.19
```


### xfsdump

> Each XFS filesystem is labeled with a Universal Unique Identifier (UUID). The UUID is stored in every allocation group header and is used to help distinguish one XFS filesystem from another, therefore you should  avoid  using `dd(1)` or other block-by-block copying programs to copy XFS filesystems.
>
> …
>
> **`xfsdump(8)` and `xfsrestore(8)` are recommended for making copies of XFS filesystems.**


### File attributes

The XFS filesystem supports setting the following file attributes on Linux systems using the [`chattr(1)`][man-chattr] utility:

`a` - append only  
`A` - no atime updates  
`d` - no dump  
`i` - immutable  
`S` - synchronous updates  

















## Example setup

   - number of drives: `n` = `3`
   - RAID level: `l` = `0`
   - XFS filesystem label: `-L` = `fs`
   - `mount` point: `/fs`

Change `n` and RAID level `l` to suit your needs.

> [!Important]
> Disk names below should be sourced from `/dev/disk/by-id/`  
🡢 Select names with a unique **serial_number** or `eui`

1. Install `mdadm` — **m**ultiple **d**evices **adm**inistration.

   ```sh
   sudo apt install mdadm
   ```

1. Create RAID level `0` virtual device `md0` with `3` disks.

   ```sh
   sudo mdadm -Cv /dev/md0 -l0 -n3 \
   /dev/disk/by-id/nvme-Samsung_SSD_XXX_PRO_1TB_<serial_number> \
   /dev/disk/by-id/nvme-Samsung_SSD_XXX_PRO_1TB_<serial_number> \
   /dev/disk/by-id/nvme-Samsung_SSD_XXX_PRO_1TB_<serial_number>
   ```

1. Check the array.

   ```sh
   cat /proc/mdstat
   sudo mdadm --detail /dev/md0
   ```

1. Format to XFS.

   ```sh
   sudo mkfs.xfs -L fs /dev/md0
   ```

1. Mount the RAID virtual device to a newly created (`-m` for mkdir) directory `/fs` .

   ```sh
   sudo mount -mo defaults,noatime,logbsize=256k /dev/md0 /fs
   ```

1. Then to setup boot mount, get the RAID virtual device UUID.

   ```sh
   sudo blkid | grep md0
   ```

   > 🡳 stdout
   >
   > ```
   > /dev/md0: LABEL="fs" UUID="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" BLOCK_SIZE="512" TYPE="xfs"
   >                            🡡                                  🡡
   >                            This is your UUID number.
   > ```

1. Copy that UUID number.

1. Open `fstab`.

   ```
   sudo nano /etc/fstab
   ```
   
1. Add this line, presumably last.

   ```
   UUID=<your UUID number> /fs xfs defaults,noatime,logbsize=256k 0 0
   ```

1. **Reboot** to check that it works.  

> [!Info]
> After reboot, the `mdadm` RAID virtual device MAY be given a different name by **udev**.  
*In my case, it's `md127` instead of `md0`.*

From now on, we'll alias this filesystem mountpoint to `$NX_FS`: adjust the path for you if needed.

```sh
NX_FS='/fs'
NX_FS_LABEL='fs'
```





### Longer version


1. Identify target disks. Notice unique names:

   ```sh
   l /dev/disk/by-id
   ```

   You may see files like this:

   ```
   … nvme-eui.e8238fa6bf530001001b448b4566aa1a -> ../../nvme0n1
   … nvme-eui.002538570142d716 -> ../../nvme1n1
   … nvme-WDS100T1X0E-00AFY0_21455A801268 -> ../../nvme0n1
   … nvme-Samsung_SSD_970_EVO_Plus_2TB_S4J4NJ0N704064T -> ../../nvme1n1
   ```

   The symlinks tell you which is which: here we have two drives (`nvme0n1` & `nvme1n1`), with *two ways* to uniquely identify each: 
   - *either* by `eui`
   - *or* by vendor + model + serial number
   
   The latter is much more convenient to maintain (notably locate the physical disk).
   
1. Put these names in a zsh variable.

   ```zsh
   disks=(
   '/dev/disk/by-id/nvme-WDS100T1X0E-00AFY0_21455A801268'
   '/dev/disk/by-id/nvme-WDS100T1X0E-00AFY0_235409743934'
   '/dev/disk/by-id/nvme-WDS100T1X0E-00AFY0_294037618901'
   )
   ```

1. Create the RAID `0` with .

   ```zsh
   sudo mdadm -Cv /dev/md0 -l0 -n${#disks[@]} "${(@)disks}"
   ```

   - `-n${#disks[@]}`: Specifies the number of devices in the RAID, dynamically determined by the number of elements in the `disks` array.
   - `"${(@)disks}"`: Expands the array elements into a space-separated string that `mdadm` can use.

1. Check up. <= Do this often! Log it!

   ```sh
   cat /proc/mdstat
   sudo mdadm --detail /dev/md0
   ```

> [!Warning]
> After reboot, the `mdadm` RAID virtual device MAY have a different name, e.g. to `md127` instead of `md0`.
>
> Quick-n-dirty fix: do `sudo mdadm --detail /dev/md*`

1. Format the RAID device.

   ```sh
   sudo mkfs.xfs -L data /dev/md0
   ```

1. Mount the RAID device to a new directory.

   ```sh
   sudo mount -mo noatime,logbsize=256k /dev/md0 /mnt/data
   ```

1. Setup mounting at boot time.

   ```sh
   sudo blkid | grep md0
   sudo nano /etc/fstab
   ```

   ```
   UUID=<your-uuid> /mnt/data xfs defaults,noatime,logbsize=256k 0 0
   ```

   Note that I'm not sure about the `largeio` option yet; testing required.



