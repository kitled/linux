# ZFS Share

Share ZFS filesystems over the network.




## Overview





## NFS

### Naive setup

1. First [install NFSD](../../../Net/Share/NFS.md#installation) (NFS server) on the system.  

    ```sh
    sudo apt install nfs-kernel-server         
    ```

    You don't need to edit `/etc/exports`.

1. Set the ZFS property `sharenfs` to `on` on the desired pool or dataset.

    ```sh
    sudo zfs set sharenfs=on mypool
    ```

1. If using a firewall, open your firewall for port `2049` (`nfs` service).  
For instance with `ufw`:

    ```sh
    sudo ufw allow nfs
    ```

    or, for better security, restrict client IP.

    ```sh
    sudo ufw allow from <CLIENT_IP> to any port nfs
    ```

### Verify

1. Check that it's done.

    ```sh
    sudo zfs get sharenfs mypool    # omit pool name to apply to all datasets
    ```

1. Check NFS shares on the system.

    ```sh
    sudo showmount -e `hostname`
    sudo exportfs -v
    ```


