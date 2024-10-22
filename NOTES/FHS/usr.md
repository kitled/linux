# `/usr`



In Linux systems, the `/usr` directory is a key component of the Filesystem Hierarchy Standard (FHS), which defines the structure and directory contents of Unix-like systems. Understanding how to properly organize software within this structure is crucial for system compatibility and maintainability. Here’s a breakdown of how software typically gets organized in `/usr` and the role of `/usr/local`:

## Standard Organization

- **`/usr/bin`**: This directory contains executable files (binaries) for user programs. If your software has a binary executable that users will run, it should be placed here.

- **`/usr/sbin`**: Contains binary files for system administration. If your software includes executables that are intended for use by system administrators, they should go here.

- **`/usr/lib`**: This directory holds the libraries needed for the binaries in `/usr/bin` and `/usr/sbin`.

- **`/usr/share`**: This is for architecture-independent (shared) data. Here you would place documentation, configuration files, and other data that do not change depending on the architecture. For example, if your software has a set of static files (like templates, icons, documentation), they should be placed in `/usr/share/<softname>/`.

- **`/usr/include`**: Contains header files used by the C compiler to build applications.




## `/usr/local`


### Role and Usage of `/usr/local`

- **`/usr/local`** is intended for software that is installed from source, or software that is not included with the distribution and is not managed by the distribution's package manager. The directory structure under `/usr/local` typically mirrors that of `/usr`:
  - **`/usr/local/bin`** - User binaries
  - **`/usr/local/sbin`** - System binaries
  - **`/usr/local/lib`** - Libraries
  - **`/usr/local/share`** - Architecture-independent data
  - **`/usr/local/include`** - Header files

### When to Use `/usr/local`

- **Third-Party or Locally Compiled Software**: Use `/usr/local` for software that is specific to the machine and not provided by the operating system's package manager. For example, if you compile software from source, it should typically be installed to `/usr/local` to avoid conflicts with software managed by the package manager.

- **Avoiding Overwrites**: Installing software in `/usr/local` ensures that updates or upgrades to the system software do not overwrite custom-installed software.

- **System Preference**: The system typically prioritizes `/usr/local/bin` over `/usr/bin` for executables, meaning that if a user installs a newer version of a program into `/usr/local/bin`, it will be used instead of the version in `/usr/bin`.






## Best Practices

- **Conform to FHS**: Stick to the Filesystem Hierarchy Standard to ensure compatibility and predictability.
- **Package Manager vs. Manual Installation**: Use the package manager when possible for installations to manage updates and dependencies automatically. Use `/usr/local` for manual installations or when specific software versions are needed that are not available via the package manager.
- **Documentation**: Document any deviations from standard practices, especially if you install software in non-standard locations.

By adhering to these conventions, you ensure that your software is organized in a manner consistent with other Unix-like systems, which aids in maintenance and user familiarity.








