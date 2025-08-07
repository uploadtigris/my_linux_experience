Date: 8/6/25
Issue: The FiiO btr5 min AMP/DAC will not work over cable.

Resolution: 

- Fedora mainly uses Pipewire. Even though they see Pipewire as superior technically to Pulseaudio, it will not work for my usecase. 
- So, I will need to install pipewire-pulse as a service to handle exporting audio to my device.
- [Follow these instructions](https://idroot.us/install-pulseaudio-fedora-42/), I used method one and it worked for me.

The ouput bitrate is stuck at 48K. Meanwhile, I know that this AMP/DAC device is capable of a higher bitrate.

1. Find the Default Config
``` bash
pactl info | grep "Sample Specification"
```
For me this returns: "Default Sample Specification: float32le 2ch 48000Hz"
PulseAudio configuration is set to 32-bit float, 2-channel, 48kHz.

2. We need to go into the config file for Pipewire and change the default audio setting
```
mkdir -p ~/.config/pipewire
```
```
cp /usr/share/pipewire/pipewire.conf ~/.config/pipewire/
```
Edit the config. Use nano or vim, whatever floats your boat
```
nano ~/.config/pipewire/pipewire.conf
```
modify the *context.properties* section
```
context.properties = {
    default.clock.rate = 192000
    default.clock.quantum = 1024
    default.clock.min-quantum = 32
    default.clock.max-quantum = 2048
}
```
save & exit

restart pipewire
```
systemctl --user restart pipewire pipewire-pulse
```
check the new settings
```
pactl info | grep "Sample Specification"
```

System works and sounds massively better! 🎧 🥁 🤘 😈
