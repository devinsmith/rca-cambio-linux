# Linux on the RCA Cambio 10.1 Tablet

![alt text](https://github.com/devinsmith/rca-cambio-linux/raw/master/images/rca_cambio.jpg "Picture of RCA Cambio tablet going from Windows to Linux")

# Distro support

Generally any Linux distribution will work with some alteration. Very few
distros currently work out of the box on this tablet due to the use of a 32
bit firmware on an amd64 Atom CPU.

As of 2024, the most recent versions of the compatible distros support most
of the hardware out-of-the-box: keyboard, display, screen rotation, Wifi,
Bluetooth, audio and battery management. However, as of May 2024 there is
[no working support for the cameras](https://askubuntu.com/questions/1250560/why-doesnt-my-camera-work-when-theres-a-kernel-module-for-the-driver)
(i.e. the OV2680 sensor) yet.

## Debian

If installing Debian 11.9 or older, use the
[images including firmware packages](https://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/),
otherwise (at least) Wifi won't work.

### Troubleshooting

- If the installer can't connect to your Wifi network, select "Enter SSID manually"
  and type the network name. If it still doesn't connect, try switching to an open
  network (or vice-versa).
- If your Wifi connection keeps dropping and not coming back, restart the
  chip service with `sudo systemctl restart rtl8723bsbt.service`.
- On Debian 12.x, after you log in audio is usually muted and routed to the headphones.
  You can reset its state back by selecting the headphones, and then the speakers in
  the sound widget, or with `alsaucm -c bytcr-rt5640 set _verb HiFi set _enadev Speaker`.
- Usually when the tablet enters in suspended mode it doesn't wake up. Disable
  "Automatic Suspend" and "Power Button Behavior" in the Power section in Settings.

### Known issues

- The Wifi connection is pretty unstable on Debian 12.x, requiring the chip service to
  be restarted often.
- On Debian 12.x you may see a flood of messages saying "ASoC: no backend DAIS enabled
  enable for ... Port" on the console. This
  [bug will be fixed](https://gitlab.freedesktop.org/pipewire/pipewire/-/issues/3407)
  in a future update and doesn't affect the audio experience.

## Ubuntu

All [Ubuntu versions with EFI boot](https://old-releases.ubuntu.com/releases/) 
(since Ubuntu 10) are meant to run on 64 bit firmware, thus unmodified installers
won't boot up. To work around that you can:

- Use Linuxium's
  [isorespin.sh](http://linuxiumcomau.blogspot.com.au/2017/06/customizing-ubuntu-isos-documentation.html)
  to create a bootable installer image.
- Use Linuxium's
  [isorespinner.sh](https://linuxiumcomau.blogspot.com/2022/04/customizing-ubuntu-isos-documentation.html)
  to create a bootable installer image.
- Use [Ventoy](https://github.com/ventoy/Ventoy) to create a bootable USB
  stick that can boot the original, unmodified images.

### Troubleshooting

- If the installer powers off the computer while in the splash screen, try disabling
  the SOC board sensor in the BIOS (in the Advanced tab). You can reenable it
  back after you finish the installation.
- Always connect to Wifi during installation so the installer can download the
  32-bit boot binaries, otherwise the installer will fail at the end while
  installing GRUB.
- If the Ubuntu 22 installer freezes constantly, try installing just the
  base/minimum system and no additional firmware/media formats.
- On Ubuntu 22, if using Gnome on Wayland (default) the sound widget won't
  show, the sound settings will be disabled and you'll have no audio. Using
  Gnome on Xorg, audio works fine (to select Gnome on Xorg, click the gear at the
  right bottom corner after selecting your user; your choice will be remembered).
- If your Wifi connection keeps dropping and not coming back, restart the
  chip service with
  `sudo modprobe -rv r8723bs ; sleep 5 ; sudo modprobe r8723bs ; sudo systemctl restart NetworkManager`.
  Usually a single restart fixes the issue.

### Known issues

- As of May 2024, the Ubuntu 24 installer crashes frequently, never finishing
  the installation, and currently there's no option to upgrade from Ubuntu 22
  to Ubuntu 24.
- Ubuntu 20 has no audio working, and it also inverts both the video orientation
  (even with the display orientation fix explained below) and the mouse cursor,
  making it hard to use in graphic mode.

## Void Linux

Need to create the custom ISO with Void's mklive and [PR102](https://github.com/voidlinux/void-mklive/pull/102).

## [OpenBSD](openbsd/)

Not a Linux distro but includes support for bootia32.efi on the install media (install61.fs).

## [Fedora 35](https://download.fedoraproject.org/pub/fedora/linux/releases/35/Workstation/x86_64/iso/Fedora-Workstation-Live-x86_64-35-1.2.iso)

# Hardware

- CPU: Intel®️ Atom™️ quad-core CPU Z3735F @ 1.33 GHz
- RAM: 2GB
- Storage: 32GB eMMC, microSD slot (up to 64GB)
- Audio: Realtek ALC5642
- Wifi: Realtek RGN RTL8723BS Wireless LAN 802.11 b/g/n 2.4GHz SDIO Network Adapter
- Bluetooth: Realtek RGN RTL8723BS (Bluetooth 4.0)
- Battery: 2 x 3.7V batteries (unknown capacity, "up to 6 hours battery life"), with AXP288 power management IC
- Display: 10.1", 1280x800 pixels, Intel HD integrated graphics
- Touchscreen: SileadTouch 10-finger multi-touch
- Camera: 1MP (front camera) and 2MP (rear camera), both using the OV2680 sensor
- Ambient light sensor: Solteam JSA-1212
- Gyroscopic sensor: Bosch BMG160
- Acceleration/compass sensor: AK09911C8KXCJK-1013
- Audio output: mono speaker, 3.5mm combo audio jack
- Video output: micro HDMI
- USB: micro USB 2.0 port, USB 2.0 port
- Weight: 2.13 lbs
- Dimensions: 10.2" (W) x 0.39" (D) x 6.5" (H)
- FCC ID: A2HW101

# Configuration

## Display

By default the RCA Cambio runs in portrait mode. To change it to landscape mode, edit `/etc/default/grub` and update `GRUB_CMDLINE_LINUX_DEFAULT`:

- if you're using only the text terminal, append `video=efifb fbcon=rotate:1`.
- if you're using a desktop environment, append `video=DSI-1:panel_orientation=right_side_up`.

Update GRUB by running `sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg` on Fedora or `sudo update-grub` on Debian or Ubuntu.

Other [display orientations](https://docs.kernel.org/fb/modedb.html) are possible.

You can confirm your video display by:

1. Getting the card devices by executing `ls /sys/class/drm`.
2. Finding the one for your display, running `cat /sys/class/drm/<card device name here>/modes` to see the resolution (800x1280) and determining which is which (mine was `card0-DSI-1`).

### Known issues and limitations

Currently there's [no way to rotate the screen](https://www.gnu.org/software/grub/manual/grub/grub.html#gfxmode) in GRUB.

Linux Kernel 4.11 and higher is needed for DRM video acceleration.

# Similar pages

- [Install Linux on Nextbook Flexx Baytrail tablet](https://github.com/burzumishi/linux-baytrail-flexx10)

