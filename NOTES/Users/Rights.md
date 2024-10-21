# Rights

A.k.a. file permissions, privileges, mode, access…





## Mode

Default Linux permissions management in octal, over 3 bits.




### Octal value

`bit` (decimal) = `letter` role  
`001` (1) = `x` e**x**ecute  
`010` (2) = `w` **w**rite  
`100` (4) = `r` **r**ead  

| # | bits | text
|---|------|-----
| 0 | `000` | `---`
| 1 | `001` | `--x`
| 2 | `010` | `-w-`
| 3 | `011` | `-wx`
| 4 | `100` | `r--`
| 5 | `101` | `r-x`
| 6 | `110` | `rw-`
| 7 | `111` | `rwx`

This octal digit is known as "mode" of a file.  
The most common that make sense are 4~7. 

> [!Note]
> When not `0` (no access),
> 
> - Readable directories are executable by principle.  
> Thus mode **`5`(`r-x`) = read-only**, or **`7`(`rwx`) = writable**.  
>
> - Files are generally **`6`(`rw-`) = writable**, or **`4`(`r--`) = read-only**;  
> executables **`5`** or **`7`** (like directories).

The above table is not optimally readable, so the table below is more useful.

| Action |`chmod`| file       | directory
|--------|-------|------------|-----------
|No access|`-rwx`| `0`(`---`) | `0`(`---`) 
| Read   | `+r`  | `4`(`r--`) | `5`(`r-x`)
| Write  | `+w`  | `6`(`rw-`) | `7`(`rwx`)
| Execute| `+x`  | `7`(`rwx`) | -




### User category

Linux considers three origins of access for a file, three categories of users, in this order (first matched is what applies).

1. `owner` user
2. `group` users
3. `other` users

Octal modes (`0`~`7`) thus come in triplets, one for each category of user: for the owner, for the group, and finally for others.

> [!Tip]  
> **`640` (`rw- r-- ---`)** for instance means:  
> `owner` user can **read** and **write**;  
> `group` users can **only read**;  
> `other` users cannot even see.

The most common modes are:

- `644`\[`755`\] ☛ owner can `read`+`write`\[+`execute`\], other can `read`.  
`rw-r--r--`\[`rwxr-xr-x`\]  
*Typical **default***

- `666`\[`777`\] ☛ everyone can `read`+`write`\[+`execute`\]  
`rw-rw-rw-`\[`rwxrwxrwx`\]  
***Free** for all*

- `600`\[`700`\] ☛ only owner can `read`+`write`\[+`execute`\].  
`rw-------`\[`rwx------`\]  
*Highly **secured***




### Mask

The `umask` (user file-creation mode mask) is a setting that determines the default permission settings for newly created files and directories.

It acts as a set of permissions that users are prevented from setting on newly created files. It effectively subtracts permissions from the system default, which is typically `666` for files (read and write) and `777` for directories (read, write, and execute).

For example, if the umask is set to `022`, new files will have the permissions `644` (`666` minus `022`) and directories will have `755` (`777` minus `022`). This means new files are readable and writable by the owner, and readable by others, but not writable or executable by others.





## ACL

If you need extended rights attributes, to manage multi-user systems, consider using POSIX ACL (Access Control List).

A POSIX ACL (Access Control List) is an extended set of file attributes that lets administrators define additional user and group rights. ACLs are strictly additive, meaning they *add* rights ("whitelist"), *never deny*.

> [!Important]
> Default ACLs applied to a directory do not affect the contents in the directory which already exists.  
> Only newly written files, **after the following procedure has been done** to the dataset or directory, will benefit from these features.  
> Copying existing files on the same dataset (duplicate them) will do the trick; then delete the old copy.




### Rationale

A fundamental limitation of the mode system is that it only allows one group to be granted rights. If you want to say, have two groups respectively able to read-only and read+write, you're out of luck. Using ACL solves the inherent complexity of even simple, common organization and architecture constrains.

By leveraging the same foundational principles as the mode however, ACL simply extends that logic with no learning curve.




### Example: give access to a second `group`

https://blog.alt255.com/post/posix-acls/

The following procedure demonstrates adding rights for a group named `media` to a dataset that belongs to another group. Adapt to suit your own needs.

> [!Tip]  
> ACL on ZFS datasets (enabled with `zfs set acltype=posixacl $DATASET_NAME` ) should be used along with its companion property `xattr=sa` which lets ZFS store these attributes in the inode (improves performance by reducing IO).
> 
> [Do it first](../Storage/FS/ZFS/Share.md#permissions), before setting up ACL below.

1. Install the `acl` package.

    ```sh
    sudo apt-get install acl
    ```


1. The `getfacl` program lists ACLs on a file or directory.  
For example, here we list the permissions of the current directory using `ls` and see the `u=rwx,g=rx,o=rx` permission bits.

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

New files and directories created will automatically have read/write/execute permissions to all members of the `media` group.


> [!Tip]  
> One neat feature of `setfacl` is reading ACLs from `stdin`:
> 
> ```sh
> getfacl srcdir | setfacl -R --set-file=- dstdir
> ```













