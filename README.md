# Network Speakers
It's worth noting that this is the config I use personally, so it's extremely opinionated. It provides AirPlay and PulseAudio over the network, and nothing else, because that's all I use.

If you have a Raspberry Pi (or ten) with a single audio outpout plugged into a stereo, it will work great. More complicated setups may get more complicated results, but if you need to manage a handful of speakers under that simple configuration, you may find this playbook extremely useful.

(I use a Pi Zero and a USB DAC.)

The most opinionated things this playbook will do:

- modify the systems hostname
- overwrite /etc/asound.conf with an extremely simple config
- install PulseAudio and run it [in system mode](https://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/User/SystemWide/)
- configure shairport-sync to provide AirPlay via PulseAudio
- disable power saving on your pi's wifi (often necessary for good streaming performance)

# Requirements
The playbook will try to automatically determine some networking variables. As a result, the machine you run ansible from, but not the Raspberry Pi target, will need the python netaddr module installed.

If you do not wish to install this module, you can just specify the allowed_networks variable as shown below.

On Fedora:

```
# sudo dnf install python3-netaddr
```

On Debian:
```
# sudo apt install python3-netaddr
```

Or with pip:

```
# pip install netaddr
```

# Variables
Two important variables can be configured. hostname is required. The hostname will be used to identify the speaker from an iOS or PulseAudio client.

The other, allowed_networks will configure PulseAudio's ACL. If not specified, we will attempt to use the speaker's network address.

Here's an example overriding both:

```
# ansible-playbook -i 192.168.155.235, -u pi -e allowed_networks="192.168.1.0/24;10.10.0.0/24" -e hostname="myspeaker01" playbook.yml
```

# Inventory File
Provision or update several speakers at once using an inventory file. Modify inventory.ini.example and run ansible like this:

```
ansible-playbook -i inventory.ini -u pi playbook.yml
```
