# Usb Sound
 A custom-made USB sound card for Balanced Audio.

# Why?

My current USB DAC isn't great and instead of just buying a new one I thought I'd try and waste some serious engineering hours learning how to build one.

# Why are you using X?

To be clear: I really have no idea what I'm doing so my choice of parts or circuit are driven by the last thing I've happened to learn.

### Why the STM32F4 Micro?

It happened to be the chip used in [Phil's video](https://www.youtube.com/watch?v=C7-8nUU6e3E) where learned to use Kicad and it happens to support I2S.

### Why the [PCM1795](https://www.ti.com/lit/ds/symlink/pcm1795.pdf) DAC?

I like the low distortion and noise floor it provides and since I'm starting with a blank canvas why not start with something that's overkill? I can only make it worse.

Note: the _audible_ differences between this and other DACs is ultimately going to be nil so there will be no notable benefit. I'm doing this because I can.

### Why the [NE5524](https://www.ti.com/lit/ds/symlink/ne5534.pdf) Op-Amps?

The PCM1795 uses that as a baseline for the measured circuits so it's a helpful starting place. I will probably switch these out for [LM4562's](https://www.ti.com/lit/ds/symlink/lm4562.pdf) in the near future once I test out the board and do initial measurements to compare against spec. The LM4562's 

Again, I can only make things worse.

### Why Balanced Audio?

The speakers I'll be driving are studio monitors and they require a balanced XLR connection.

## Are you going to support multiple output channels?

Eventually, probably. The micro supports 3 I2S lines, and each I2S line supports two channels out and one in.

## Are you going to support audio in?

Maybe. See the output section.

## What's the drivers story?

Ideally it'll just show up as a USB audio device and all major operating systems will just pick it up.

![](doc/UsbSound.png?raw=true)
