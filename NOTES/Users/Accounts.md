# Accounts

Manage `user` accounts on Linux systems.

> [!Tip]
> - See [Rights](Rights.md) to manage user permissions ("mode") like `sudo` access and file `rwx` privileges.  
> - See [Groups](Groups.md) as a complement to `user` features.

## Overview

The concept of "users" is a base feature (and requirement) of most operating systems (OS):  all actions (commands) must be performed by a known actor, called `user`.

A `user` account contains at least a `username`, `password`, and generally a `home` directory wherein to place all files related to this user.

> [!Tip]
> When the system is accessed from the network (from another system, e.g. through [SSH](../SSH), the remote actor must assume a local user identity. When serving resources (like responding to HTTP requests), the software operating the system (for instance FastHTML) will be executed by a local user.

On Linux, all users belong to at least one group with the same name. So, user `john` is part of the `john` group, which is created at the same time as the user account.

All UNIX/Linux systems begin with a user account called `root`, homed at `/root`, who has full rights ("privileges") on the entire system, including the meta-rights to change rights for all users.

The general task of system administrators (humans, like you and yours, truly) is to organize a sane separation of concerns between other user accounts who have ad hoc rights to do whatever they do. 

> [!Tip]
> The mental idea is that `root` is not a real person, it is the system itself, unhindered; and we all only borrow ad hoc root rights for ad hoc purposes.
> The core mantra is that having least privileges necessary for the task yields the strongest security, as all system functions are tightly gated behind credentials, out of reach from unauthorized actors.





## Procedures









