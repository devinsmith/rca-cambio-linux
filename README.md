# Linux on the RCA Cambio 10.1 Tablet

![alt text](https://github.com/devinsmith/rca-cambio-linux/raw/master/images/rca_cambio.jpg "Picture of RCA Cambio tablet")

# Distro support

Generally any Linux distribution will work with some alteration. Very few
distros currently work out of the box on this tablet due to the use of a 32
bit firmware on an amd64 Atom CPU.

- Debian (using [mixed mode](https://cdimage.debian.org/mirror/cdimage/release/9.0.0/multi-arch/iso-cd/ "Mixed mode ISO"))
- Ubuntu (using Linuxium's [isorespin.sh](http://linuxiumcomau.blogspot.com.au/2017/06/customizing-ubuntu-isos-documentation.html))
- Void (needs custom ISO)

  Need to create the custom ISO with Void's mklive and
  [PR102](https://github.com/voidlinux/void-mklive/pull/102)

- OpenBSD, not a Linux distro but included support for bootia32.efi on the
  install media.

# Hardware

TBD

# Similar pages


