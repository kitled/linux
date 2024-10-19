# Btrfs


> [!Caution]
> **NEVER use RAID 5 or 6 with Btrfs!**  
> It's ***fundamentally* broken**.


We use [`block-group-tree`](https://btrfs.readthedocs.io/en/latest/mkfs.btrfs.html#filesystem-features) to *"greatly reduce mount time for large filesystems."*

Note that we'll have to [disable COW](https://wiki.archlinux.org/title/Btrfs#Disabling_CoW) for the VM image directory using `chattr +C /path/to/dir` to avoid a useless performance hit.




Great blog: [Forza's ramblings](https://wiki.tnonline.net/w/Category:Btrfs)

- https://wiki.tnonline.net/w/Category:Btrfs
- https://wiki.tnonline.net/w/Btrfs/Getting_Started
- https://wiki.tnonline.net/w/Btrfs/Features
- https://wiki.tnonline.net/w/Btrfs/Profiles
- https://wiki.tnonline.net/w/Blog/NFS_on_Btrfs
- https://wiki.tnonline.net/w/Blog/The_case_for_(no)_atime_on_Linux
- https://wiki.tnonline.net/w/Blog/SQLite_Performance_on_Btrfs
- https://wiki.tnonline.net/w/Btrfs/Deduplication
- https://wiki.tnonline.net/w/Btrfs/Compression
- https://wiki.tnonline.net/w/Btrfs/Zstd

ext

- https://ftp.gnu.org/gnu/glibc/
- https://manpages.debian.org/unstable/zstd/zstd.1.en.html


