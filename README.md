# Linux on the RCA Cambio 10.1 Tablet

![alt text](https://github.com/devinsmith/rca-cambio-linux/raw/master/images/rca_cambio.jpg "Picture of RCA Cambio tablet")

# Distro support

Generally any Linux distribution will work with some alteration. Very few
distros currently work out of the box on this tablet due to the use of a 32
bit firmware on an amd64 Atom CPU.

- Debian (using [mixed mode](https://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/10.6.0+nonfree/multi-arch/iso-cd/ "Mixed mode ISO"))
- Ubuntu (using Linuxium's [isorespin.sh](http://linuxiumcomau.blogspot.com.au/2017/06/customizing-ubuntu-isos-documentation.html))
- Void (needs custom ISO)

  Need to create the custom ISO with Void's mklive and
  [PR102](https://github.com/voidlinux/void-mklive/pull/102)

- OpenBSD, not a Linux distro but includes support for bootia32.efi on the
  install media (install61.fs).

# Hardware

- CPU: Intel(R) Atom(TM) CPU Z3735F @ 1.33 GHz
- Battery: axp288 (min/max: 0x??/0x??)
- Wifi: Realtek RTL8723BS Wireless LAN 80211n SDIO Network Adapter
- TBD

# Configuration

By default the RCA Cambio runs in portrait mode, but before booting you'll want
to add `video=efifb fbcon=rotate:1` to your kernel command line.

Linux Kernel 4.11 and higher is needed for DRM video acceleration.

# Similar pages

- [Install Linux on Nextbook Flexx Baytrail tablet](https://github.com/burzumishi/linux-baytrail-flexx10)

