# Filesystem Hierarchy Standard (FHS)

The **Filesystem Hierarchy Standard** (**FHS**) defines the directory structure and directory contents in Unix-like operating systems. It is designed to ensure consistency and predictability in file system layouts across different systems, making it easier to manage software installations, system operations, and upgrades.

## Overview of key directories


### Essential System Directories

- **`/bin`**: Essential user command binaries.[^usrmerge]
- **`/sbin`**: Essential system binaries.
- **`/lib`**: Essential shared libraries and kernel modules.
- **`/etc`**: Host-specific system configuration.


### User and Administrative Directories

- **`/home`**: Home directories for regular users.
- **`/root`**: Home directory for the root user.


### Variable Data Directories

- **`/var`**: Variable files like logs and caches.
- **`/tmp`**: Temporary files.


### Kernel and Device Directories

- **`/dev`**: Device files.
- **`/proc`**: Virtual filesystem providing process and kernel information as files.
- **`/sys`**: Virtual filesystem providing detailed information about kernel and devices.


### Secondary Hierarchy for User Data

- **`/usr`**: Contains the majority of user utilities and applications:
    - **`/usr/bin`**: User command binaries that are not required for booting or repairing the system.
    - **`/usr/sbin`**: Non-essential system binaries, mainly used for system administration (not needed by regular users for basic operations).
    - **`/usr/lib`**: Libraries for the binaries in `/usr/bin` and `/usr/sbin`.
    - **`/usr/local`**: Tertiary hierarchy for local data specific to the host. This includes locally installed software that should not interfere with the system software managed by the distribution's package manager.


### Application and Service Data Directories

- **`/opt`**: Optional add-on applications.
- **`/srv`**: Data for services provided by the system.


### Boot and Mount Directories

- **`/boot`**: Static files for the boot loader.
- **`/mnt`**: Mount point for mounting a filesystem temporarily.
- **`/media`**: Mount point for removable media.








[^usrmerge]: As part of a broader movement known as the "usrmerge" project, which aims to simplify the filesystem hierarchy by consolidating directories that historically were separate due to technical limitations of earlier Unix systems, these folders are now usually merged with their `/usr` counterpart.

    ```
    bin -> usr/bin
    sbin -> usr/sbin
    
    lib -> usr/lib
    lib64 -> usr/lib64
    ```


