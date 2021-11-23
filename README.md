# Linux on the RCA Cambio 10.1 Tablet

![alt text](https://github.com/devinsmith/rca-cambio-linux/raw/master/images/rca_cambio.jpg "Picture of RCA Cambio tablet")

# Distro support

Generally any Linux distribution will work with some alteration. Very few
distros currently work out of the box on this tablet due to the use of a 32
bit firmware on an amd64 Atom CPU.

- Debian (using [mixed mode](https://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/10.6.0+nonfree/multi-arch/iso-cd/ "Mixed mode ISO"))
- Ubuntu (using Linuxium's [isorespin.sh](http://linuxiumcomau.blogspot.com.au/2017/06/customizing-ubuntu-isos-documentation.html))
- Void Linux
- [OpenBSD](openbsd/), not a Linux distro but includes support for bootia32.efi on the
  install media (install61.fs).
- [Fedora 35](https://download.fedoraproject.org/pub/fedora/linux/releases/35/Workstation/x86_64/iso/Fedora-Workstation-Live-x86_64-35-1.2.iso)

# Hardware

- CPU: Intel(R) Atom(TM) CPU Z3735F @ 1.33 GHz
- Battery: axp288 (min/max: 0x??/0x??)
- Wifi: Realtek RTL8723BS Wireless LAN 80211n SDIO Network Adapter
- TBD

# Configuration

By default the RCA Cambio runs in portrait mode, but before booting you'll want
to add `video=efifb fbcon=rotate:1` to your kernel command line.

To get out of portrait mode by default in Fedora,
1. Get card devices by executing `ls /sys/class/drm`
2. Find the one for your display, you can run `cat /sys/class/drm/<card device name here>/modes` to see the resolution and determine which is which (mine was `card0-DSI-1`)
3. Use Grubby to add the following kernel arguments (using my example above, note that `left_side_up` is also an option): `sudo grubby --update-kernel=ALL --args="video=DSI-1:panel_orientation=right_side_up"`

Linux Kernel 4.11 and higher is needed for DRM video acceleration.

# Similar pages

- [Install Linux on Nextbook Flexx Baytrail tablet](https://github.com/burzumishi/linux-baytrail-flexx10)

