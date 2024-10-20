# Music Player Daemon (MPD)

[MPD](https://www.musicpd.org/) (Music Player Daemon)






## Installation

Great post to setup `mpd`: https://www.24bit96.com/hifi-music-server/bitperfect-linux-with-mpd.html

Assuming you have a Debian or Ubuntu box up and running, you only need a few steps.

1. Install ALSA tools

    ```sh
    sudo apt install alsa-utils alsa-tools 
    ```

1. Open `/etc/security/limits.conf`

    ```sh
    sudo nano /etc/security/limits.conf
    ```

1. Add the following lines

    ```
    @audio - rtprio 99
    @audio - memlock unlimited
    @audio - nice -10
    ```

    Save: <kbd>Ctrl</kbd>+<kbd>o</kbd> then <kbd>Enter</kbd>  
    Close the editor: <kbd>Ctrl</kbd>+<kbd>x</kbd>  

1. Install `mpd`

    ```sh
    sudo apt install mpd
    ```

1. Assign `mpd` to the group `audio`

    ```sh
    sudo adduser mpd audio
    ```

1. Make the directory for your music and playlist files

    ```
    sudo mkdir -p /home/usbaudio/share/{music,playlist}
    ```

1. Set write permissions for all

    >*like, really? no other way?..*

    ```
    sudo chmod -R 777 /home/usbaudio/share
    ```


## Audio configuration

>[!Warning]
> There is an issue here, because the numbers below might change between reboots.  
> 
>
> Ways to solve this issue:
> - At boot (`systemd` unit to `enable` in `systemctl`), run some script that 
>     - fetches the correct id from `/proc/asound/cards`
>     - `sed` that into `/usr/share/alsa/alsa.conf`
> - Ideally, **before `mpd` starts**, using `systemd` units `before` and `after` statements.  
> - If editing the `mpd` service unit is not possible for some reason, then 
>     - set it to `disable` in systemd
>     - trigger `start mpd` ourselves in our own systemd unit (with the soundcard matching thing)

1. Make sure the USB DAC is connected. If it wasn't already, shutdown, plug it in, then boot.  
This totally caveman move helps ensuring you get the 'normal' id below, until we fix this method.

1. List audio devices.

    ```sh
    sudo aplay --list-devices
    ```

1. You get a list of devices like this:

    ```
    **** List of PLAYBACK Hardware Devices ****
    card 0: NVidia [HDA NVidia], device 3: HDMI 0 [HDMI 0]
      Subdevices: 1/1
      Subdevice #0: subdevice #0
    ...
    card 1: Generic_1 [HD-Audio Generic], device 3: HDMI 0 [SAMSUNG]
      Subdevices: 1/1
      Subdevice #0: subdevice #0
    ...
    ```

    You're looking for the one that matches your USB DAC.  
    *In my case for the Topping 10s, it's `card 3`.*

    ```
    card 3: D10 [D10], device 0: USB Audio [USB Audio]
      Subdevices: 0/1
      Subdevice #0: subdevice #0
    ```

1. Alternatively, you can just look at `/proc/asound/cards` to confirm the card number.  

    ```sh
    sudo cat /proc/asound/cards
    ```

    ```
     0 [NVidia         ]: HDA-Intel - HDA NVidia
                          HDA NVidia at 0xdf080000 irq 219
     1 [Generic_1      ]: HDA-Intel - HD-Audio Generic
                          HD-Audio Generic at 0xdf588000 irq 221
     2 [Generic        ]: HDA-Intel - HD-Audio Generic
                          HD-Audio Generic at 0xdf580000 irq 222
     3 [D10            ]: USB-Audio - D10
                          Topping D10 at usb-0000:67:00.0-5, high speed
    ```

    *Again, `3` for me.*

1. Use the `card` number above in `/usr/share/alsa/alsa.conf` as shown below: I changed these values from `0` to `3`.

    ```
    defaults.ctl.card 3
    defaults.pcm.card 3
    defaults.pcm.device 3
    ```


## MPD configuration

1. Open `/etc/mpd.conf`.

    ```sh
    sudo nano /etc/mpd.conf
    ```

1. Here are the lines to modify. 

    You want to uncomment all the things hereby required, comment out some (like the input section).

    ```
    # Files and directories #######################################################
    music_directory         "/home/usbaudio/share/music"
    playlist_directory      "/home/usbaudio/share/playlist"
    db_file                 "/var/lib/mpd/tag_cache"
    log_file                "/var/log/mpd/mpd.log"
    pid_file                "/var/run/mpd/pid"
    state_file              "/var/lib/mpd/state"
    sticker_file            "/var/lib/mpd/sticker.sql"
    # General music daemon options ################################################
    user                            "mpd"
    group                          	"audio"
    # For network
    #bind_to_address                "localhost"
    port                            "6600"
    auto_update     				"yes"
    # Symbolic link behavior ######################################################
    follow_outside_symlinks        "yes"
    follow_inside_symlinks        "yes"
    # Zeroconf / Avahi Service Discovery ##########################################
    zeroconf_enabled               "yes"
    zeroconf_name                  "debianmusic"
    # Input #######################################################################
    #input {
    #       plugin "curl"
    #       proxy "proxy.isp.com:8080"
    #       proxy_user "user"
    #       proxy_password "password"
    #}
    # Audio Output ################################################################
    audio_output {
            type            "alsa"
            name            "My ALSA Device"
            device          "hw:1,0"        # optional
            mixer_type      "disabled"      # optional
    #       mixer_device    "default"       # optional
    #       mixer_control   "PCM"           # optional
    #       mixer_index     "0"             # optional
    }
    # Character Encoding ##########################################################
    filesystem_charset              "UTF-8"
    ###############################################################################
    ```

1. I personally added this, notably for streaming to M.A.L.P. (Android client). It's not great, though, I'll find a better solution soon™.

    ```
    save_absolute_paths_in_playlists    "yes"

    ...

    metadata_to_use "+comment"

    ...

    audio_output {
            type            "httpd"
            name            "My HTTP Stream"
            encoder         "opus"
            port            "8080"
            signal          "music"
            bitrate         "max"
    }

    ```

1. Finally start, enable, and check that it went fine.

    ```sh
    sudo systemctl enable mpd
    sudo systemctl start mpd
    sudo systemctl status mpd
    ```

    If you made an error in the config file, it'll tell you in the log at the bottom of the `status` screen.

    You may also check the global system journal directly (where the above `status` mini-log comes from). 

    ```sh
    journalctl  # --help for lots of options, notably -xf
    ```

If all went fine, you may now use [clients](#clients) to control MPD.

> [!Tip]
> Debugging crash course
>
> Add `-xf` flags to watch the journal in reverse order (last entry first) and in real time, to witness what happens with anything afterwards.
> 
> ```sh
> journalctl -xf
> ```
> 
> Then, in another terminal:
> 
> ```sh
> sudo systemctl reload-or-restart mpd
> ```
> 
> You should see any error happening in real time in the journal.  
> That's where your debugging journey begins, with clues found here. Copy/paste those to some search or AI and see what comes up: you're likely not the first to encounter this particular issue.





### Clients

https://www.musicpd.org/clients/



#### Local

To check whether things work locally, `mpc` works. It's unmaintained though.

`mpc` is a GUI classic for a quick spin locally. It's GTK though, so ugly on KDE. \>\_\<  
I'd use that to test the setup, and then go on my merry quest to find the greatest apps.

```sh
sudo apt install mpc
```




#### Android

- https://gitlab.com/gateship-one/malp
- https://github.com/abarisain/dmix



