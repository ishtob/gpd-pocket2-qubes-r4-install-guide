1. Download Qubes r4.0 from the official site and setup a bootable USB

2. attach the qubes install usb and boot into the installer.

3. the installer will be rotated as the screen is portrait by default. press ctrl+alt+F2 to go into TTY and type in ` echo 1 | sudo tee /sys/class/graphics/fbcon/rotate` the display should now be in the correct orientation. press ctrl+alt+F6 to return to the graphical installer. Follow the installer and install qubes as normal.

5. DO NOT reboot once install is completed, enter TTY (ctrl + alt + F2) and type `echo 1 > /sys/class/graphics/fbcon/rotate_all` to rotate screen.

6. find and copy down the uuid of your encrypted drive using `blkid /dev/mmcblk0p2` it should be right after `UUID="`

7. go into `vi /mnt/sysimage/boot/EFI/qubes/xen.cfg` and add the following to the line starting with `kernel=...`
` fbcon:rotate=1 vga=791`
*** read below for full drive encryptions ***
optional: currently I have not found a way to patch plymouth to rotate to the screen orientation, so if you care about the decryption dialog being sideways, you hide the graphical interface by removing `rhgb` from this line

9. type in `reboot` to boot into the newly installed qubes and finish setting up the VMs.


***********
if you are using full drive encryption:

During step 8: replace the value after `rd.luks.name=` with the uuid you had written down earlier and end with `=now`*. press esc, then type `:wq` to exit.Next, enter `vi /mnt/sysimage/etc/crypttab` and replace the first line withh `now`* followed by the UUID from earlier, then `none` and add `discard` if you wish to use trim

* `now` can be any name you wish to name the drive, I personally like `now` as the decrypt prompt would say `Enter passphrase to unlock the disk now!:`

***********


*** Touchscreen orientation

1. Use an appVM (ie. untrusted) and download the following it as zip https://github.com/stockmind/gpd-pocket-screen-indicator 
2. Open a terminal in dom0 and use command line to copy `gpd-pocket-screen-indicator-master.zip` from the appVM to dom0(please understand the security concerns with putting scripts in dom0, and review the code used for rotation to ensure your security, instructions on how to do this can be found here: https://www.qubes-os.org/doc/copy-from-dom0/)

3. unzip `gpd-pocket-screen-indicator-master.zip` then `cd gpd-pocket-screen-indicator-master` and run the the install script in root using `sudo ./install.sh`

4. reboot and the touchscreen should now follow the orientation of the screen. additional steps are needed to get the widget to show up.


***********


*** Hold right click to scroll with opticalmouse

create `nano /etc/X11/xorg.conf.d/20-scroll-emu.conf' and paste in

```
Section "InputClass"
  Indentifier "optical scroll emulation"
  MatchIsPointer  "on"
  MatchProduct  "sys-usb: HAILUCK CO.,LTD USB KEYBOARD"
  Driver  "libinput"
  Option "ScrollMethod" "button"
  Option "ScrollButton" "3"
EndSection
```
reboot to take effect
