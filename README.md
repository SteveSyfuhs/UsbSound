# Usb Sound
 A custom-made USB sound card for Balanced Audio.

# Why?

My current USB DAC isn't great and instead of just buying a new one I thought I'd try and waste some serious engineering hours learning how to build one instead.

# What's wrong with the current DAC?

My existing DAC uses the [PCM2705](https://www.ti.com/lit/ds/symlink/pcm2705.pdf) chip to handle both the USB and DAC side of things. It's fine, I guess, but it has some annoying quirks.

First, you have to bolt on an additional chip to do silly things like control the USB descriptors. Absent that you're left with this annoying string with two trailing spaces (probably nulls?). Clearly not the end of the world, but I might as well aim for perfect.

![image](https://user-images.githubusercontent.com/1210849/114077591-d69da180-985c-11eb-8ab9-c4dd4436a0ce.png)

Second, the output from the chip is only single-ended so it requires extra effort to make a _good_ differential output. This existing device is using a transformer (two, on per channel) to generate the differential pair, which is great for isolation, but transformers are notorious for distortion at high frequency. 

Curiously this device also doesn't produce a great differential output at 1000hz.

![image](https://user-images.githubusercontent.com/1210849/114078378-b3272680-985d-11eb-854e-d0bc20287a09.png)

Channel A (blue) is the positive leg and Channel B (red) is the negative leg. Channel 3 (magenta?) is A+B, which ideally should sum to zero, not ~16mV RMS.

# Why are you using X?

To be clear: I really have no idea what I'm doing so my choice of parts or circuit are driven by the last thing I learned about. If there's clearly a better way to do something then please let me know.

### Why the STM32F4 Micro?

It happened to be the chip used in [Phil's video](https://www.youtube.com/watch?v=C7-8nUU6e3E) where learned to use Kicad and it happens to support I2S. It's incredibly powerful for the use case here, and a less powerful device could be used instead. That said, it could make for a useful platform for building out digital filters and stuff.

### Why the [PCM1795](https://www.ti.com/lit/ds/symlink/pcm1795.pdf) DAC?

I like the low distortion and noise floor it provides and since I'm starting with a blank canvas why not start with something that's overkill? I can only make it worse.

Note: the _audible_ differences between this and other DACs is ultimately going to be nil so there will be no notable benefit. I'm doing this because I can.

### Why the [NE5524](https://www.ti.com/lit/ds/symlink/ne5534.pdf) Op-Amps?

The PCM1795 uses that as a baseline for the measured circuits so it's a helpful starting place. I will probably switch these out for [LM4562's](https://www.ti.com/lit/ds/symlink/lm4562.pdf) in the near future once I test out the board and do initial measurements to compare against spec. The LM4562s have lower distortion and noise floor, and more interestingly contain two op-amps in the package so I can reduce the circuit footprint.

Again, I can only make things worse.

### Why Balanced Audio?

The speakers I'll be driving are studio monitors and they require a balanced XLR connection.

## Are you going to support multiple output channels?

Eventually, probably. The micro supports 3 I2S lines, and each I2S line supports two channels out and one in.

## Are you going to support audio in?

Maybe. See the output section.

## What's the driver story?

Ideally it'll show up as a USB audio device and all major operating systems will just pick it up.

# Measurement Methodology

A work in progress. Ideal testing should measure:

 * Noise - Specifically what the device adds to the signal.
 * Distortion - Specifically how the device modifies the signal integrity across the device bandwidth.
 * Flatness - Specifically whether the device has amplified or attenuated voltage across the device bandwidth.

![](doc/UsbSound.png?raw=true)
