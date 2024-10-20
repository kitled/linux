# NFS

🐧 https://wiki.debian.org/NFSServerSetup

🅰️ https://wiki.archlinux.org/title/NFS






## Server




### Installation

🐧`Debian` https://wiki.debian.org/NFSServerSetup

Make sure you have NFS server support in your server's kernel (kernel module named "`knfsd.ko`" or "`nfsd.ko`" under your `/lib/modules/uname -r/` directory structure). The config file will confirm if the kernel was compiled with NFS enabled.

```sh
grep NFSD /boot/config-`uname -r`
```
🖥️`stdout`
    ⮟
```
CONFIG_NFSD=m
# CONFIG_NFSD_V2 is not set
CONFIG_NFSD_V3_ACL=y
CONFIG_NFSD_V4=y
CONFIG_NFSD_PNFS=y
CONFIG_NFSD_BLOCKLAYOUT=y
# CONFIG_NFSD_SCSILAYOUT is not set
# CONFIG_NFSD_FLEXFILELAYOUT is not set
# CONFIG_NFSD_V4_2_INTER_SSC is not set
CONFIG_NFSD_V4_SECURITY_LABEL=y
```


#### Kernel-space

Install the kernel packages.[^out1]

```sh
sudo apt install nfs-kernel-server         
```

> [!Tip]
> If [sharing from ZFS][zfs-share], you're done here: no need to edit `/etc/exports`.

> [!Note]
> For Debian, additional configuration information can be found in this old admin page (still recommended in the Debian wiki apparently): https://www.debianadmin.com/network-file-system-nfs-server-and-client-configuration-in-debian.html

☛ Pay attention to some recommendations, like `systemclt mask` for the RPC Bind thing normally unneeded for NFSv4.  
After a wild reboot caused by a power outage (server wasn't on a UPS at that time…), the `nfs-server.service` would fail until I `systemctl unmask` both `rpcbind.service` and `rpcbind.socket`



## Client

🅰️ https://wiki.archlinux.org/title/NFS#Client_configuration

### Installation

```sh
sudo apt install nfs-common
```

### Mounting

For NFSv4 mount the root NFS directory and look around for available mounts:

```sh
sudo mount servername:/ /mountpoint/on/client
```

Then mount omitting the server's NFS export root:

```sh
sudo mount -t nfs -o vers=4 servername:/music /mountpoint/on/client
```

### autofs

https://wiki.archlinux.org/title/Autofs#NFS_network_mounts

AutoFS provides automatically discovering and mounting NFS-shares on remote servers (the AutoFS network template in `/etc/autofs/auto.net` has been removed in autofs5). 

1. To enable automatic discovery and mounting of network shares from all accessible servers without any further configuration, simply edit the `auto.master` as follows.  
It will auto-mount NFS hosts into `/net`.

    📄`/etc/autofs/auto.master`
    ```
    /net -hosts --timeout=60
    ```

> [!Note]
> Each host name needs to be resolveable, e.g. the name and IP address in /etc/hosts or via DNS and make sure you have `nfs-utils` installed and configured. You also have to enable `rpcbind` to browse shared directories.

1. For a remote server `fileserver` (the name of the directory is the hostname of the server), with an NFS share named `/home/share`, you can now just access the share by that path.

```sh
cd /net/fileserver/home/share
```

> [!Note]
> Please note that *ghosting* (i.e. automatically creating directory placeholders before mounting shares) is enabled by default; although, AutoFS installation notes claim to remove that option from `/etc/conf.d/autofs` in order to start the AutoFS daemon.

The `-hosts` option uses a similar mechanism as the `showmount` command to detect remote shares. You can see the exported shares by typing:

```sh
sudo showmount servername -e
```

#### Manual NFS configuration

To mount a NFS share for `file_server` on `/srv/shared_dir` at location `/mnt/foo`, add a new configuration, e.g. `file_server.autofs`.

📄`/etc/autofs/auto.master.d/file_server.autofs`  
```
/mnt   /etc/autofs/auto.file_server --timeout 60
```

📄`/etc/autofs/auto.file_server`  
```
foo  -rw,soft,rsize=8192,wsize=8192 file_server:/srv/shared_dir
```

---

**Footnotes**

[^out1]: 🖥️`stdout` for `sudo apt install nfs-kernel-server`  
    ⮟

    ```
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    The following additional packages will be installed:
      keyutils libevent-core-2.1-7 libnfsidmap1 libyaml-0-2 nfs-common python3-yaml rpcbind
    Suggested packages:
      open-iscsi watchdog
    The following NEW packages will be installed:
      keyutils libevent-core-2.1-7 libnfsidmap1 libyaml-0-2 nfs-common nfs-kernel-server python3-yaml rpcbind
    0 upgraded, 8 newly installed, 0 to remove and 0 not upgraded.
    Need to get 872 kB of archives.
    After this operation, 3,342 kB of additional disk space will be used.
    Do you want to continue? [Y/n] 
    ```





[^man]: https://manpages.debian.org/stretch/nfs-kernel-server/rpc.nfsd.8.en.html



[zfs-share]: ../../Storage/FS/ZFS/Share.md


