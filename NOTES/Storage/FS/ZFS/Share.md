# ZFS Share

Share ZFS filesystems over the network.




## Overview

This note only contains the parts relevant to ZFS specifically; generic procedures can be found at [Net](../../../Net/) > [Share](../../../Net/Share/) > [NFS](../../../Net/Share/NFS.md).



## NFS Server

### Naive setup

1. First [install NFSD](../../../Net/Share/NFS.md#installation) (NFS server) on the system.  

    ```sh
    sudo apt install nfs-kernel-server         
    ```

    You don't need to edit `/etc/exports` for ZFS sharing.


1. Set the ZFS property `sharenfs` to `on` on the desired pool (here `big`) or dataset.

    ```sh
    sudo zfs set sharenfs=on big
    ```

    Children datasets inherit properties by default, including share.  
    E.g., `big/things` will also be shared.


1. If using a firewall, open your firewall for port `2049` (`nfs` service).  
For instance with `ufw`:

    ```sh
    sudo ufw allow nfs
    ```

    or, for better security, restrict client IP.

    ```sh
    sudo ufw allow from <CLIENT_IP> to any port nfs
    ```


1. Check that it's done.

    ```sh
    sudo zfs get sharenfs mypool    # omit pool name to apply to all datasets
    ```


1. Check NFS shares on the system.

    ```sh
    sudo showmount -e `hostname`
    sudo exportfs -v
    ```


### Permissions

Normally, use Linux default mode rights ( `chown` and `chmod` ).

If you need extended rights attributes, to manage multi-user systems, consider using POSIX ACL (Access Control List).

> [!Tip]
> See User > [ACL][acl] for a comprehensive explanation.

https://blog.alt255.com/post/posix-acls/

The following procedure demonstrates adding rights for a group named `media` to a dataset that belongs to another group. Adapt to suit your own needs.


1. Set `xattr=sa` and `acltype=posixacl` on the dataset.

    ```sh
    zfs set xattr=sa big/things
    zfs set acltype=posixacl big/things
    ```

See User > [Rights](../../../Users/Rights.md#example-give-access-to-a-second-group) to setup ACL.







## NFS Client

### Naive mount

> ```sh
> export         POOL_NAME="big"
> export       SERVER_NAME="srv"    # in /etc/hosts or DNS
> export   POOL_MOUNTPOINT="/$POOL_NAME"
> export CLIENT_MOUNTPOINT="/net/$POOL_NAME"
> ```

```sh
sudo mount $SERVER_NAME:$POOL_MOUNTPOINT $CLIENT_MOUNTPOINT
```

> ```sh
> sudo mount srv:/big /net/big
> ```

> [!Tip]  
> Using <kbd>Tab</kbd> for auto-completion lets you validate inputs as-you-go.  
For instance above: commands and options, variable names, and paths including network mounts.


### Auto-mount

`autofs` will auto-mount either *all* available shares on the network (one-liner in default config), or a specified subset thereof.

See [Net](../../../Net/)/[Share](../../../Net/Share/)/[NFS](../../../Net/Share/NFS.md) #[**autofs**](../../../Net/Share/NFS.md#autofs) for a comprehensive explanation.

































[acl]: ../../../Users/Rights.md#acl