# Systemd



## Overview

🔗 [Arch wiki](https://wiki.archlinux.org/title/Systemd)  
🔗 [Debian wiki](https://wiki.debian.org/systemd) (quoted below)  

> Systemd is a system and service manager for Linux. It is the default init system for Debian since Debian 8 ("jessie"). Systemd is compatible with SysV and LSB init scripts. It can work as a drop-in replacement for sysvinit. Systemd
> 
> - Provides aggressive parallelization capabilities
> - Uses socket and D-Bus activation for starting services
> - Offers on-demand starting of daemons
> - Implements transactional dependency-based service control logic
> - Tracks processes using Linux cgroups
> - Supports snapshotting and restoring
> - Maintains mount and automount points
> - Systemd runs as a daemon with PID 1.
> 
> Systemd tasks are organized as units. The most common units are services (.service), mount points (.mount), devices (.device), sockets (.socket), or timers (.timer). For instance, starting the secure shell daemon is done by the unit ssh.service.
> 
> Systemd puts every service into a dedicated control group (cgroup) named after the service. Modern kernels support process isolation and resource allocation based on cgroups.
> 
> Targets are groups of units. Targets call units to put the system together. For instance, graphical.target calls all units that are necessary to start a workstation with graphical user interface. Targets can build on top of another or depend on other targets. At boot time, systemd activates the target default.target which is an alias for another target such as graphical.target.
> 
> Systemd creates and manages the sockets used for communication between system components. For instance, it first creates the socket /dev/log and then starts the syslog daemon. This approach has two advantages: Firstly, processes communicating with syslog via /dev/log can be started in parallel. Secondly, crashed services can be restarted without processes that communicate via sockets with them losing their connection. The kernel will buffer the communication while the process restarts.












## `man`

📘 [`systemd` index](https://manpages.debian.org/bookworm/systemd/index.html)


