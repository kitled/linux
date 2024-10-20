# ZFS Share

Share ZFS filesystems over the network.




## Overview





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

https://blog.alt255.com/post/posix-acls/

1. Set `xattr=sa` and `acltype=posixacl` on the dataset.

    ```sh
    zfs set xattr=sa big/things
    zfs set acltype=posixacl big/things
    ```

1. Install `acl`.

    ```sh
    sudo apt-get install acl
    ```


1. The `getfacl` program lists ACLs on a file or directory. For example, here we list the permissions of the current directory using `ls` and see the `u=rwx,g=rx,o=rx` permission bits.

    ```sh
    ls -ld .
    ```

    ```
    drwxr-xr-x 64 jburke jburke 140 Feb  4 18:02 .
    ```


1. `getfacl` gives more verbose output.

    ```sh
    getfacl .
    ```  
    🡳
    ```
    # file: .
    # owner: jburke
    # group: jburke
    user::rwx
    group::r-x
    other::r-x
    ```


1. To enable the `media` group to automatically have read, write, and execute permissions to new directory entries, we use the `setfacl` program.

    ```sh
    setfacl -d -m g:media:rwx .

    getfacl .
    ```
    🡳
    ```
    # file: .
    # owner: jburke
    # group: media
    user::rwx
    group::r-x
    other::r-x
    default:user::rwx
    default:group::r-x
    default:group:media:rwx
    default:mask::rwx
    default:other::r-x
    ```


Notice the `default:group:media` line indicating that the `media` group has `rwx` permission by default.

For my immediate goals, this was mission accomplished: new files and directories created would automatically have read/write/execute permissions to all members of the `media` group.

Once I was satisfied that ACLs were working as needed on my test directory, I used `getfacl` and `setfacl` to transfer the setings to another directory. One feature of `setfacl` is reading ACLs from `stdin`:

```sh
getfacl srcdir | setfacl -R --set-file=- dstdir
```

> [!Note]
> Default ACLs applied to a directory do not affect the contents in the directory which already exists.






































