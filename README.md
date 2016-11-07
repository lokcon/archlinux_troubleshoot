# archlinux_troubleshoot
A personal repository of various problems encountered when using Arch Linux

## Unsolved problems
* bluetooth

## Unsolved Problems (Macbook Air 2013)
### Cannot adjustscreen brightness
### Switching of fn fx keys
### Touchpad settings in Gnome Shell
### Close lid to sleep
### Blank screen after wake up
### Power Savings
* Install powertop
* Install tlp


### Gcin not working in some applications
#### Description
Gcin seems only to work in firefox and cinnamon UI
#### (Possible) Solution
After removing some gnome packages bunbled with gnome/cinnamon, gcin suddenly works again.

It still does not work in the newest version of Spotify.

### Touchpad not configurable in Cinnamon
#### Description
Touchpad cannot be diabled and configured through settings in Cinnamon
#### (Possible) Solution
Touchpad is indeed working through ```libinput``` driver, and can be configured and disabled through command-line utilities.
Maybe work that out with Cinnamon / Replace libinput with Synaptics driver?

## Solved Problems

### New version of Spotify does not integrate with Gnome-ish DE nicely
#### Description
    * There is no task bar item for Spotify
    * The Indicator panel does not work   
    * Lanuching Spotify opens another instance alongside already-running ones
#### (Possible) Solution:
Maybe try using older version of the client that comes with gnome support
http://repository-origin.spotify.com/pool/non-free/s/


### Mounting NTFS Drives
#### Description
NTFS drives mounted even with ```mount``` are mounted with ```drwx------ root root``` permission, resulting normal users unable to write to the drives.
#### Solution
Use [ntfs-3g](https://wiki.archlinux.org/index.php/NTFS-3G)


### Authentication required for mounting drives in graphical interfaces (nautilus, nemo etc.)
#### Solution
https://bbs.archlinux.org/viewtopic.php?id=102050


### Change gnome-terminal to alternatives in Cinnamon
#### Solution
https://bbs.archlinux.org/viewtopic.php?id=152157

Create file ```/etc/polkit-1/rules.d/10-auth.rules``` with content:
```javascript
 polkit.addRule(function(action, subject) {
               if (action.id="org.freedesktop.udisks2.filesystem-mount-system" && subject.isInGroup("storage")) {
                       return polkit.Result.YES;
                   }
               }
           );
```

### Use (or not use) Nemo to show desk icons
#### Solution
`gsettings set org.nemo.desktop show-desktop-icons false`

https://wiki.archlinux.org/index.php/Nemo#Show_.2F_hide_desktop_icons

### A2DP Sink does not load for bluetooth headset
### Description
When a bluetooth headset is connected to the system, a2dp_sink failed loading. A result of this is the sound quality is very bad and mono.
### Solution

https://bbs.archlinux.org/viewtopic.php?id=197482
```
Bug and possible solution: actually I found a bug in that make the headset unusable, it seems that the pulse audio module: module-bluetooth-discover works only if started after the X11 session is up. So I have a workaround.

Edit the file:
/etc/pulse/default.pa
and comment out (with an # at the beginning of the line) the following line:
#load-module module-bluetooth-discover

now edit the file:
/usr/bin/start-pulseaudio-x11
and after the lines:
   if [ x”$SESSION_MANAGER” != x ] ; then
        /usr/bin/pactl load-module module-x11-xsmp “display=$DISPLAY session_manager=$SESSION_MANAGER” > /dev/null
    fi

add the following line:
    /usr/bin/pactl load-module module-bluetooth-discover

This way the Pulse audio’s Bluetooth modules will not be downloaded at boot time but after x11 is started.
```

## Solved Problems (Macbook Air 2013)

This section specific to Macbook Air 7,2 which came early 2015

### Bluetooth Headset cannot be connected
#### Solution
(from https://m157q.github.io/posts/2015/09/10/install-arch-linux-on-macbook-air-mid-2013/)
https://wiki.archlinux.org/index.php/Bluetooth
https://wiki.archlinux.org/index.php/Blueman
https://wiki.archlinux.org/index.php/Bluetooth_headset
* `sudo pacman -S bluez bluez-utils bluez-libs bluez-firmware blueman pulseaudio-bluetooth pulseaudio-alsa pavucontrol`
* `sudo modprobe btusb`
* `sudo systemctl enable bluetooth`
* `sudo systemctl start bluetooth`
* `blueman-manager`
    * scan, pair, connect
* `pavucontrol`
    * Change sound channel to bluetooth headset
* Sometimes the bluetooth device may be blocked
    * use `rfkill list` to check if it is blocked.
    * use `sudo rfkill unblock bluetooth` to unblock.


### Wireless
#### Solution (for system installation)
Consider using an Android to USB tether internet

#### Solution
Install [broadcom-wl](https://aur.archlinux.org/packages/broadcom-wl/) from **AUR**
Reboot might be required for the driver to work

https://wiki.archlinux.org/index.php/broadcom_wireless

### No Sound after installing alsa-utils
#### Solution
https://wiki.archlinux.org/index.php/MacBook#Audio

Add the following to `/etc/asound.conf`
```
defaults.pcm.card 1
defaults.pcm.device 0
defaults.ctl.card 1
```
No reboot is required.
