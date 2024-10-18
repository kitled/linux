# Audio

Two scenarios: 

1. Local (think power outlets, home listening, hifi experience)
1. Streaming (think mobile, roaming, car, radio-like)









## Local playback

Stack: "**MAD**" (MPD + ALSA + DAC)

1. [**MPD**](mpd.md) (Music Player Daemon) handles library files, metadata and playback
1. ➥ to [**ALSA**](https://www.alsa-project.org/wiki/) (Advanced Linux Sound Architecture) 
1. ➥ to USB [**DAC**](DAC.md) (Digital to Analog Converter)

Clients (web, PC, mobile…) are used for control.




### Overview

Local playback is static: a fixed machine hooked up to audio equipment. There is not a single wireless part in the audio chain itself, whose bits go straight from the file to the chip that converts them to analog signals and finally amplified to speakers. 

Such setups should yield "bit-perfect" audio for the best theoretical quality, and strive to take the shortest, most transparent path from file to ear.

In this scenario, we maximize quality—notably dynamic range and latency—at the expense of other considerations—such as bitrate (ultimately storage space) or power used.


### Music Player Daemon (MPD)

https://www.musicpd.org/

> Music Player Daemon (MPD) is a flexible, powerful, server-side application for playing music. Through plugins and libraries it can play a variety of sound files while being controlled by its network protocol.

See [note](mpd.md) for install & config.





## Streaming

Stack: ???

1. ?

### Overview

In contrast to static local "hifi" playback, streaming has opposite requirements.  
- Noisy environments and crappy speakers turn dynamic range into an enemy more than your best friend as in the living room.  
You want 'loudness' compression (just not a brick either), and perhaps a slight EQ push in the mid range (or roll-off of both highs and lows) to improve clarity.
- Storage, bandwidth, compute, and power are in limited quantity.
- There's a lot of reencoding needed to stream a rich library (with varied & high resolution formats). Different strategies will suit different needs (from just-in-time to fully pre-reencoded, passing by prediction and caches).

