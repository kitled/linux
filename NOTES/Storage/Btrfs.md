# Btrfs


> [!Caution]
> **NEVER use RAID 5 or 6 with Btrfs!**  
> It's ***fundamentally* broken**.


We use [`block-group-tree`](https://btrfs.readthedocs.io/en/latest/mkfs.btrfs.html#filesystem-features) to *"greatly reduce mount time for large filesystems."*

Note that we'll have to [disable COW](https://wiki.archlinux.org/title/Btrfs#Disabling_CoW) for the VM image directory using `chattr +C /path/to/dir` to avoid a useless performance hit.




Great blog: [Forza's ramblings](https://wiki.tnonline.net/w/Category:Btrfs)


