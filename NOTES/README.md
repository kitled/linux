# Linux Notes

<!--

> [!Note]
> Unlike per-file notes, to be found throughout this repository *except* in this directory at the same location of the file itself on a Linux system, these notes are titled and intended at doing a **job**, **solution** a particular problem, **actionable procedure**.
> 
> They involve by essence multiple tools and locations.
> 
> It's hard to find a one-size-fits-all structure, so it may evolve in time, but discoverability will be handled by tags in the metadata of Markdown files.

-->

## Table of contents

A directory note is the `./README.md`  
All READMEs = full toc & intros of these notes


> [!Warning]
> This list is a WIP / TODO!
> A lot may be incorrect or missing at any time.
> 
> 
> `.`  
> `├──` [**Shell**](Shell)  
> `│   ├──` Bash  
> `│   └──` [Zsh](Shell/ZSH)  
> `├──` [**SSH**](SSH)  
> `│   ├──` [`config`](SSH/config.md)  
> `│   ├──` [`ssh-keygen`](SSH/ssh-keygen.md)  
> `│   ├──` [`sshd`](SSH/sshd.md) (OpenSSH Server)   
> `│   └──` [SCP](SSH/SCP.md) (Secure copy protocol)  
> `├──` [**Storage**](Storage)  
> `│   ├──` [Btrfs](Storage/Btrfs.md)  
> `│   ├──` [XFS](Storage/XFS.md)  
> `│   └──` [ZFS](Storage/ZFS)  
> `├──` [**Systemd**](Systemd)  
> `│   ├──` [Services](Systemd/Services.md)  
> `│   └──` [Timers](Systemd/Timers.md)  
> `└──` [**Users**](Users)  
> `    ├──` [Accounts](Users/Accounts.md)  
> `    ├──` [Groups](Users/Groups.md)  
> `    └──` [Rights](Users/Rights.md)  

<!-- TEMPLATE

.  
`├──` zxcv  
`│    ├──` zxcv  
`│    ├──` zxcv  
`│    └──` zxcv  
`├──` zxcv   
`│    └──` zxcv 
`└──` zxcv    
`     ├──` zxcv  
`     ├──` zxcv   
`     └──` zxcv  

-->

## Visual abstract

This doesn't look great in my opinion. Will redo better using something else.

```mermaid
graph LR

NS[Namespaces]
FS[Filesystems]

Linux --- Apps & Media & Storage & Network & Shell & System & Test & Users & misc

Media --- Text & Pictures & Audio & Video
Audio --- mpd & DAC

Storage --- NS & FS & Tools
FS --- ZFS & Btrfs & XFS

Network --- Share --- NFS

Shell --- Zsh & SSH & multiplex

System --- Systemd

Test --- stress-ng

Users --- Accounts & Groups & Rights
```



## Variables

List of variables used in these notes.  

> [!Tip]
> The use of variables as placeholders in deliberate: it helps differentiating between actual commands and user-chosen strings.
>
> For instance, in the command below, it's clear that you should choose the value of the variables `$POOL_NAME` and `$DATASET_NAME`, whereas `sudo zfs create` is used verbatim.
> 
> > ```sh
> > export POOL_NAME="hyperpool"
> > export DATASET_NAME="hyperset"
> > ```
> ```sh
> sudo zfs create $POOL_NAME/$DATASET_NAME
> ```
>
> For clarity in procedures as in the above example, variables declarations are indented within a quote block, before the command using them.











