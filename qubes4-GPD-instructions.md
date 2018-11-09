1. Download Qubes r4.0 from the official site and setup a bootable USB

2. attach the qubes install usb and boot into the installer.

3. the installer will be rotated as the screen is portrait by default. press ctrl+alt+F2 to go into TTY and type in ` echo 1 | sudo tee /sys/class/graphics/fbcon/rotate` the display should now be in the correct orientation. press ctrl+alt+F6 to return to the graphical installer. Follow the installer and install qubes as normal.

5. Boot into the the qubes installer usb again, once there enter TTY and type `echo 1 | sudo tee /sys/class/graphics/fbcon/rotate` to rotate screen.

6. Type:
`sudo mkdir /mnt/boot
sudo mount mmcblk0p1 /mnt/boot`

7. find and copy down the uuid of your encrypted drive using `sudo lvs` it should start with `luks-`

8. go into `sudo vi /mnt/boot/EFI/qubes/xen.cfg` and add the following to the line starting with `kernel=...`

` fbcon:rotate=1`

if you are using full drive encryption
and replace the value after `rd.luks.uuid=luks-` with the uuid you had written down earlier. 


optional: currently I have not found a way to patch plymouth to rotate to the screen orientation, so if you care about the decryption dialog being sideways, you hide the graphical interface by removing `rhgb` from this line

8. boot into the newly installed qubes and finish setting up the VMs.


*** Touchscreen orientation

 Use an appVM (ie. untrusted) and download the following it as zip https://github.com/stockmind/gpd-pocket-screen-indicator then copy the .zip to dom0 (please understand the security concerns with putting scripts in dom0, and review the code used for rotation to ensure your security)

unzip and run the install.sh


