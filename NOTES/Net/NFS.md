# NFS

🐧 https://wiki.debian.org/NFSServerSetup

🅰️ https://wiki.archlinux.org/title/NFS

> [!Note]
> Use NFS**v4**. You know who you are if this doesn't apply to you.





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

First, the packages.[^out1]

```sh
sudo apt install nfs-kernel-server         
```

> [!Tip]
> If sharing from ZFS, you're done.  
> No need to edit `/etc/exports`.  
> You may want to setup [`ssh`](../SSH/config.md) to `send|recv`.




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
