# Audio

Two scenarios: 

1. Local (think power outlets, home listening, hifi experience)
1. Streaming (think mobile, roaming, car, radio-like)





## Local playback

Stack: "**MAD**" (MPD + ALSA + DAC)

1. [**MPD**](mpd.md) (Music Player Daemon) 
1. output to [**ALSA**](https://www.alsa-project.org/wiki/) (Advanced Linux Sound Architecture) 
1. feeding the USB [**DAC**](DAC.md) in bit-perfect playback (PCM and DSD)

Clients (web, PC, mobile…) are used for control.





### Music Player Daemon (MPD)

https://www.musicpd.org/

> Music Player Daemon (MPD) is a flexible, powerful, server-side application for playing music. Through plugins and libraries it can play a variety of sound files while being controlled by its network protocol.

See [note](mpd.md) for install & config.



## Streaming


