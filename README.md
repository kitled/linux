# Linux


Notes on all things Linux.

> [!Tip]
> Start here: [NOTES](/NOTES/README.md)

----


<!--

## How to use

> *It's just a bunch of notes* : ) *So do as you wish!*  

Here's my thinking.  
(work-in-progress)

- To see the note of any  
`/…/thing` ==> you add "`--note.md`" at the end:  
`/…/thing--note.md`

    For instance,  
    `cat /etc/systemd/system/my_service.service--note.md`  
    will display the note about `my_service.service`

- All packages managed by the package manager (like `apt`) will usually have their note at the usual location:

    ```sh
    cat /usr/bin/grep--note.md
    cat /usr/sbin/useradd--note.md
    cat /proc/cpuinfo
    cat /dev/by-id    # A dir path displays /dev/by-id/README--note.md (like GitHub)
    …
    cat /any/custom/command--note.md
    ```







<!--

- Fork this repository to add your own stuff.  

    - Do a PR if it's a general thing that other people use already. (`httpd` is OK, but not `my_super_custom_thing`)

    - I care about typos and grammar so don't hesitate to PR the most minute fixes.

- It's easy to deploy this as a website, using MKDocs, Hugo, whatever.

    - `[TODO]`  
    I'll make a FastHTML thing to unleash the power of Python >_>

- ⚠️ `[TODO]`  
Use a shell function (or simple alias) to quickly pull notes, like the `man` or `tldr` syntax but for all the things.
    ```sh
    note grep
    note chmod
    note MODE           # "Concept" in /NOTES (UUID)
    note /etc/systemd   # Displays /etc/systemd/README--note.md (like GitHub)
    note /usr/local/bin/my_command
    ```


- ⚠️ `[TODO]`  
**Once released:**  
(currently `v0.1` "writing in the open"; **wait for `v1`** to trust it),  

    You may deploy (just `cp`) this repository to `/` on a Linux system, and it will put all notes (`--note.md` files) at their correct location.

    Nothing will change on the system, as scripts/commands won't be executable by default.

    - To further secure execution, a prefix "`EXEC--`" is appended to all executable files such as `some_script.sh` or `/usr/local/bin/some_command`, which you must therefore **rename before use** (in addition to `chmod +x…` (or `755`), and `chown root…` for security). 
    
        *Otherwise, the command won't work, and systemd services files won't find the script, but you can always hack that by running* `EXEC--some_script.sh` *directly: the point is that you **know** and that it **won't happen silently or by mistake**.)*

    - Use the string `EXEC--` recursively to quickly `find` (or exclude) executable files.

    - Use the string `--note.md` recursively to quickly `find` (or exclude) note files.

-->





<!--

## Structure

1. All notes have the suffix.extension `--note.md` (Markdown file).


1. Notes are placed next to the thing:

    `/path/to/file`    
    `/path/to/file--note.md`

    All relative paths in this repo (first/second/third.ext) can be inferred from an actual Linux system.
    
    For instance, if there's a file  
    `/etc/ssh/sshd_config` on Linux,  
    and if I have a note about it,  
    then it's located in this repo at  
    `etc/ssh/sshd_config--note.md`


1. A typical Linux filesystem (here, Debian 12) has the following directories.  
    This repository mirrors this structure, placing each note next to the thing. 

    ```
    boot/       mnt/        srv/        bin -> usr/bin/
    dev/        opt/        sys/        lib -> usr/lib/
    etc/        proc/       tmp/        lib64 -> usr/lib64/
    home/       root/       usr/        sbin -> usr/sbin/
    media/      run/        var/        
    ```


1. I added a few directories.

    ```
    ADMIN/      Administrator manual
    CONFIGS/    ⚙️ Turn-key procedures & scripts
    GUIDES/     Let me explain CONFIGS
    INFO/       Not Linux but related (vendors, CSP, OEM, GH…)
    KB/         Heavy book-style exhaustive docs.

    NOTES/   📔 My notes  <==  BEGIN HERE :)
    ```


1. The most important directory is **[`NOTES`](/NOTES/README.md)**.  
**[Begin there](/NOTES/README.md)** if unsure. 


1. `ADMIN` is my "delta" manual: take all the manuals, then add mine to fill the gaps—and *hasten* the work!


1. `GUIDES` will contain more prose, discussion, perspectives, stories.


1. `CONFIGS`, explained by `GUIDES`, tie together all the notes knowledge into actionable stacks ready for production. 

    Procedures(📜) may be written as:
    
    - `.sh` shell scripts(⚙️) prefixed with `EXEC--` (rename + `chmod` to run)
    - notebooks(📑) using a `bash` kernel (requires Jupyter install + custom things)
    - `ssh` wrappers to deploy remotely (e.g., on your VPS).


1. `INFO` & `KB` are just directories to put things that would clutter elsewhere.













----

<!--


© All our wonderful contributors — Free to use and work with, just don't re-sell it, and give link|credit when citing.

-->
