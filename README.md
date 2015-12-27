# archlinux_troubleshoot
A personal repository of various problems encountered when using Arch Linux

## Unsolved problems
* bluetooth
* typing on desktop in cinnamon

#### Gcin not working in some applications
##### Description
Gcin seems only to work in firefox and cinnamon UI
##### (Possible) Solution
After removing some gnome packages bunbled with gnome/cinnamon, gcin suddenly works again.
It still does not work in the newest version of Spotify.

#### Touchpad not configurable in Cinnamon
##### Description
Touchpad cannot be diabled and configured through settings in Cinnamon
##### (Possible) Solution
Touchpad is indeed working through ```libinput``` driver, and can be confiured and disabled through command-line utilities.
Maybe work that out with Cinnamon / Replace libinput with Synaptics driver?

## Solved Problems

#### New version of Spotify does not integrate with Gnome-ish DE nicely
##### Description
    * There is no task bar item for Spotify
    * The Indicator panel does not work   
    * Lanuching Spotify opens another instance alongside already-running ones
##### (Possible) Solution:
Maybe try using older version of the client that comes with gnome support
http://repository-origin.spotify.com/pool/non-free/s/


#### Mounting NTFS Drives
##### Description
NTFS drives mounted even with ```mount``` are mounted with ```drwx------ root root``` permission, resulting normal users unable to write to the drives.
##### Solution
use [ntfs-3g](https://wiki.archlinux.org/index.php/NTFS-3G)


#### Authentication required for mounting drives in graphical interfaces (nautilus, nemo etc.)
##### Related links
https://bbs.archlinux.org/viewtopic.php?id=102050


#### Change gnome-terminal to alternatives in Cinnamon
##### Solution
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
