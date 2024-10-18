# Audio

Audio on Linux used to be a pain, and now it's just delightful.

The stack is built on [ALSA](https://www.alsa-project.org/wiki/) (Advanced Linux Sound Architecture), running [MPD](https://www.musicpd.org/) (Music Player Daemon) to an external [DAC](DAC.md) for bit-perfect playback (PCM and DSD).

## Music Player Daemon (MPD)

https://www.musicpd.org/

> Music Player Daemon (MPD) is a flexible, powerful, server-side application for playing music. Through plugins and libraries it can play a variety of sound files while being controlled by its network protocol.

Great post to setup `mpd`: https://www.24bit96.com/hifi-music-server/bitperfect-linux-with-mpd.html

Assuming you have a Debian or Ubuntu box up and running, it's simpler though.

ALSA (Advanced Linux Sound Architecture) is a free sound architecture for Linux systems and controls the USB DACs. You install ALSA with the following command

```sh
sudo apt-get install alsa-utils alsa-tools 

Edit the file "/etc/security/limits.conf" and add the following lines:

@audio - rtprio 99
@audio - memlock unlimited
@audio - nice -10


5) Installing MPD

MPD (Music Player Daemon) is the actual music server and is installed as follows:
sudo apt-get install mpd

Now assign "mpd" to the group "audio":
sudo adduser mpd audio

Make the directory for your music- and playlist-files
sudo mkdir /home/usbaudio/share /home/usbaudio/share/music /home/usbaudio/share/playlist

The write permission should be set as follows:
sudo chmod 777 /home/usbaudio/share /home/usbaudio/share/music /home/usbaudio/share/playlist



### Clients

https://www.musicpd.org/clients/


#### Android

- https://gitlab.com/gateship-one/malp
- https://github.com/abarisain/dmix



