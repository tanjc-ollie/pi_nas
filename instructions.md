How to Turn a Raspberry Pi Into a NAS for Whole-Home File Sharing (https://www.pcmag.com/how-to/how-to-turn-a-raspberry-pi-into-a-nas-for-whole-home-file-sharing)

1. Install an Operating System

2. Unmount Your Drive

  sudo fdisk -l

Find the external drive you want to use.

First, you need to unmount the drive.

  sudo umount /dev/sda1

Depending on the drive, you may need to also run umount /dev/sda2, umount /dev/sda3, and so on, depending on how many partitions are on the drive from previous usage. Then, to erase and format your flash drive for Linux usage, run:

  sudo parted /dev/sda

3. Partition Your Drive
raspberry pi partition
When you run that code, it will open up a wizard called Parted, which will allow you to create a new partition on the drive. Run this command, pressing Enter after each answer in the wizard and replacing MyExternalDrive with the name you want to use for the drive:

  mklabel gpt

If prompted to erase the drive, type Y and press Enter. Then run:

  mkpart
  MyExternalDrive
  ext4
  0%
  100%
  quit

The final Quit command will exit the Parted wizard. Obviously, you can adjust these commands to fit the name of your drive, the number and size of partitions you want to make on it, and so on—but for most basic users just starting out, these commands should work well.


4. Format the Partition
raspberry pi partition
Next, we need to format that partition. If your drive is located at /dev/sda, the new partition will be located at /dev/sda1 (if the drive is /dev/sdb, you will use /dev/sdb1, and so on). Run this code:

  sudo mkfs.ext4 /dev/sda1

Press Y and Enter when asked if you want to proceed. Then run and replace MyExternalDrive with whatever you want to name your drive:

  sudo e2label /dev/sda1 MyExternalDrive

Formatting will take a few minutes, especially if you have a large drive, so be patient. When it is finished, run this command to reboot your Pi:

  sudo shutdown -r now

How to Free Up Disk Space in Windows
  sudo chown -R pi /media/pi/MyExternalDrive


5. Share the Drive

  sudo apt update
  sudo apt upgrade
  sudo apt install samba samba-common

The installer will ask if you want to modify smb.conf to use WINS settings from DHCP. Choose Yes and press Enter. Now you edit that configuration file yourself, to share your drive. Run:

  sudo nano /etc/samba/smb.conf

  [MyMedia]
  path = /media/pi/MyExternalDrive/
  writeable = yes
  create mask = 0775
  directory mask = 0775
  public=no

6. Create a Password and Add Users

sudo smbpasswd -a pi
