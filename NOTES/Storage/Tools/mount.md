# `mount`


📘`man`: [`mount`][man-mount]

Canonical form: `mount [-fnrsvw] [-t fstype] [-o options] device mountpoint`

To display the list of all currently mounted filesystems:

```sh
mount    # you generally want to `| grep something` here
```

### Basic use

A formatted device can be mounted to a directory using `mount` as superuser.

```sh
sudo mount -vm /dev/md0 /mnt/data
```

> [!Tip]
> `-m` (`--mkdir`) creates the mount point directory if it does not exist yet.

### `fstab` (boot mount)

Get the UUID of the device:

```sh
sudo blkid | grep /dev/<your-device>
```

Edit `/etc/fstab` to add an entry for the RAID 0 array:

```sh
sudo nano /etc/fstab
```

Add this line (replace `UUID` with the actual UUID from the `blkid` command):

```fstab
UUID=<your-uuid> /mnt/data xfs defaults,noatime 0 0
```

You can mount all `fstab` entries with `mount -a`.

```sh
sudo mount -a
```













[man-mount]: https://manpages.ubuntu.com/manpages/noble/en/man8/mount.8.htmlz