# Systemd services




📙 https://wiki.debian.org/systemd/Services

📙 https://manpages.debian.org/bookworm/systemd/systemd.unit.5.en.html
























































































## `man`

📘 [`systemd.unit` | `SYSTEMD.UNIT(5)`](https://documentation.suse.com/smart/systems-management/html/systemd-management/index.html) 


### NAME

```
systemd.unit - Unit configuration
```

### SYNOPSIS

```
service.service, socket.socket, device.device, mount.mount,
automount.automount, swap.swap, target.target, path.path, timer.timer,
slice.slice, scope.scope
```


#### System Unit Search Path

```
/etc/systemd/system.control/*
/run/systemd/system.control/*
/run/systemd/transient/*
/run/systemd/generator.early/*
/etc/systemd/system/*
/etc/systemd/system.attached/*
/run/systemd/system/*
/run/systemd/system.attached/*
/run/systemd/generator/*
...
/lib/systemd/system/*
/run/systemd/generator.late/*
```

#### User Unit Search Path

```
~/.config/systemd/user.control/*
$XDG_RUNTIME_DIR/systemd/user.control/*
$XDG_RUNTIME_DIR/systemd/transient/*
$XDG_RUNTIME_DIR/systemd/generator.early/*
~/.config/systemd/user/*
$XDG_CONFIG_DIRS/systemd/user/*
/etc/systemd/user/*
$XDG_RUNTIME_DIR/systemd/user/*
/run/systemd/user/*
$XDG_RUNTIME_DIR/systemd/generator/*
$XDG_DATA_HOME/systemd/user/*
$XDG_DATA_DIRS/systemd/user/*
...
/usr/lib/systemd/user/*
$XDG_RUNTIME_DIR/systemd/generator.late/*
```


