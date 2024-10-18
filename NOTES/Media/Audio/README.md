# Audio

Audio on Linux used to be a pain, and now it's just delightful.

The stack is built on [ALSA](https://www.alsa-project.org/wiki/) (Advanced Linux Sound Architecture), running [MPD](https://www.musicpd.org/) (Music Player Daemon) to an external [DAC](DAC.md) for bit-perfect playback (PCM and DSD).

## Music Player Daemon (MPD)

https://www.musicpd.org/

> Music Player Daemon (MPD) is a flexible, powerful, server-side application for playing music. Through plugins and libraries it can play a variety of sound files while being controlled by its network protocol.

Great post to setup `mpd`: https://www.24bit96.com/hifi-music-server/bitperfect-linux-with-mpd.html

Assuming you have a Debian or Ubuntu box up and running, you only need a few steps.

1. Install ALSA tools.

    ```sh
    sudo apt-get install alsa-utils alsa-tools 
    ```

1. Open the file `/etc/security/limits.conf` .

    ```sh
    sudo nano /etc/s
    ```

1. Add the following lines.

    ```
    @audio - rtprio 99
    @audio - memlock unlimited
    @audio - nice -10
    ```

1. Install `mpd`.

    ```sh
    sudo apt-get install mpd
    ```

1. Assign `mpd` to the group `audio` .

    ```sh
    sudo adduser mpd audio
    ```

1. Make the directory for your music and playlist files.

    ```
    sudo mkdir -p /home/usbaudio/share/{music,playlist}
    ```

1. Set write permissions for all. 

>*like, really? no other way?..*

    ```
    sudo chmod -R 777 /home/usbaudio/share
    ```


### Clients

https://www.musicpd.org/clients/


#### Android

- https://gitlab.com/gateship-one/malp
- https://github.com/abarisain/dmix



