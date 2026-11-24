# Network Speakers
Basically just an AirPlay speaker.

The main thing it does is configure PipeWire to filter the audio much like the "dialog boost" will do on some TVs. At night, I don't want loud jarring noises. The filter makes quiet sounds slightly louder, and loud sounds a good deal quieter. The filter is automatically activated in the evening and deactivated in the morning.

If you have a Raspberry Pi (or ten) with a single audio output plugged into a stereo, it will work great. More complicated setups may get more complicated results, but if you need to manage a handful of speakers under that simple configuration, you may find this playbook extremely useful.

(I use a Pi Zero 2 W and a USB DAC.)

The most opinionated things this playbook will do:

- modify the system's hostname
- overwrite /etc/asound.conf with an extremely simple config
- install PipeWire, pipewire-pulse, and wireplumber and run them as a system service
- configure shairport-sync to provide AirPlay via PulseAudio
- disable power saving on your pi's wifi (often necessary for good streaming performance)
- setup PipeWire to limit/normalize audio, and automatically configure it at night

# Variables
The hostname, which will identify the speaker for Airplay, can be configured with a variable.

The timezone can also be configured. Because the filter is activated on a schedule, the timezone being correct is important.

```
# ansible-playbook -i 192.168.155.235, -u pi -e hostname="myspeaker01" -e timezone="America/Chicago" playbook.yml
```

# Inventory File
Provision or update several speakers at once using an inventory file. Modify inventory.ini.example and run ansible like this:

```
ansible-playbook -i inventory.ini -u pi playbook.yml
```
