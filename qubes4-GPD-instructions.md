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
*** read below for full drive encryptions ***
optional: currently I have not found a way to patch plymouth to rotate to the screen orientation, so if you care about the decryption dialog being sideways, you hide the graphical interface by removing `rhgb` from this line

9. boot into the newly installed qubes and finish setting up the VMs.


***********
if you are using full drive encryption:

During step 8: replace the value after `rd.luks.uuid=luks-` with the uuid you had written down earlier. 

After setp 9: open up terminal in dom0. type `ls /dev/mapper/luk*` and copy down the value after 'luks-' `sudo nano /etc/crypttab` and replace the work `none` with the value copied from the previous command.

***********


*** Touchscreen orientation

1. Use an appVM (ie. untrusted) and download the following it as zip https://github.com/stockmind/gpd-pocket-screen-indicator 
2. Open a terminal in dom0 and use command line to copy `gpd-pocket-screen-indicator-master.zip` from the appVM to dom0(please understand the security concerns with putting scripts in dom0, and review the code used for rotation to ensure your security, instructions on how to do this can be found here: https://www.qubes-os.org/doc/copy-from-dom0/)

3. unzip `gpd-pocket-screen-indicator-master.zip` then `cd gpd-pocket-screen-indicator-master` and run the the install script in root using `sudo ./install.sh`

4. reboot and the touchscreen should now follow the orientation of the screen. additional steps are needed to get the widget to show up.
