# Arch Linux Installation Guide

**Note:** Before proceeding, it is highly recommended to visit the [Arch Linux Installation Guide](https://wiki.archlinux.org/index.php/Installation_guide) on the Arch Linux Wiki. This guide may change over time, so make sure to refer to the latest instructions there.

This repository provides step-by-step instructions for installing Arch Linux on both a virtual machine and physical hardware. Follow these instructions carefully, and don't forget to consult the Arch Linux Wiki for any updates or changes in the installation process.

## Wireless Connection Setup

1. Load the keyboard layout: loadkeys es

2. Connect to your Wi-Fi network using `iwctl`. Replace `NOMBRE_DISPOSITIVO` with your device name, and `NOMBRE_ROUTER` with your router's name:
```
      iwctl
      device list
      station NOMBRE_DISPOSITIVO scan
      station NOMBRE_DISPOSITIVO get-networks
      station NOMBRE_DISPOSITIVO connect NOMBRE_ROUTER
      exit
```
3. Verify your internet connection: 

```ping archlinux.org```

4. Synchronize the system clock: 

```timedatectl set-ntp true```


## Create and Format Partitions

5. Use `cfdisk` or your preferred partitioning tool to create partitions as needed.

6. Format the partitions. Replace `/dev/sda5`, `/dev/sda6`, and `/dev/sda7` with your actual partition names:
```
      mkfs.ext4 /dev/sda5
      mkfs.ext4 /dev/sda6
      mkswap /dev/sda7
      swapon /dev/sda7
      mount /dev/sda5 /mnt
      mkdir /mnt/home
      mount /dev/sda6 /mnt/home
```

7. Mount the EFI partition (if applicable):
   
      mkdir /mnt/boot
      mount /dev/sda2 /mnt/boot


## Install the System

8. Install the base system and essential packages: pacstrap /mnt base linux linux-firmware
   
9. Generate an `fstab` file: genfstab -U /mnt >> /mnt/etc/fstab

    
## Configure the System

10. Chroot into the installed system:
 ```
 arch-chroot /mnt
 ```

11. Set the time zone. Replace `Europe/Madrid` with your desired time zone:
 ```
 ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime
 ```

12. Set the hardware clock:
 ```
 hwclock --systohc
 ```

13. Install a text editor (e.g., Nano):
 ```
 pacman -S nano
 ```

14. Edit the `locale.gen` file and uncomment the desired locales:
 ```
 nano /etc/locale.gen
 ```

15. Generate the locale files:
 ```
 locale-gen
 ```

16. Create and set the `LANG` and `KEYMAP` variables:
 ```
 echo "LANG=es_ES.UTF-8" > /etc/locale.conf
 echo "KEYMAP=es" > /etc/vconsole.conf
 ```

17. Set the hostname to your desired name (replace "asus" with your hostname):
 ```
 echo "asus" > /etc/hostname
 ```

18. Edit the `/etc/hosts` file and add your hostname:
 ```
 nano /etc/hosts
 ```

19. Set the root password:
 ```
 passwd
 ```

20. Install NetworkManager for network management:
 ```
 pacman -S networkmanager
 ```

21. Enable NetworkManager to start at boot:
 ```
 systemctl enable NetworkManager
 ```

22. Install GRUB and EFI bootloader tools (if applicable):
 ```
 pacman -S grub efibootmgr
 ```

23. Install GRUB to the EFI system partition:
 ```
 grub-install --target=x86_64-efi --efi-directory=/boot
 ```

24. Generate the GRUB configuration file:
 ```
 grub-mkconfig -o /boot/grub/grub.cfg
 ```

25. Create a user and set a password. Replace "usuario" with your desired username:
 ```
 useradd -m usuario
 passwd usuario
 ```

26. Add the user to necessary groups (replace `$USERNAME` with your username):
 ```
 usermod -aG wheel,audio,video,storage $USERNAME
 ```

27. Install sudo:
 ```
 pacman -S sudo
 ```

28. Edit the sudoers file to allow your user to use sudo:
 ```
 visudo
 ```

29. Exit the chroot environment:
 ```
 exit
 ```

30. Unmount all partitions and reboot:
 ```
 umount -R /mnt
 shutdown now
 ```

## Install KDE Plasma Desktop Environment

31. After booting into the system, connect to your Wi-Fi network again if needed:
 ```
 nmcli device wifi list
 nmcli device wifi connect NOMBRE password CONTRASEÑA
 ```

32. Verify your internet connection:
 ```
 ping archlinux.org
 ```

33. Install Xorg for graphical display:
 ```
 sudo pacman -S xorg
 ```

34. Install additional packages using the Arch User Repository (AUR) helper 'yay':
 ```
 sudo pacman -S git
 cd /opt
 sudo git clone https://aur.archlinux.org/yay-git.git
 sudo chown -R $USERNAME:$USERNAME ./yay-git
 cd yay-git
 sudo pacman -S base-devel
 makepkg -si
 sudo pacman -Syu
 yay -S wd719x-firmware aic94xx-firmware
 ```

35. Install the Plasma desktop environment and essential applications:
 ```
 sudo pacman -S plasma-meta kde-applications-meta
 ```

36. Enable the SDDM display manager to start at boot:
 ```
 sudo systemctl enable sddm
 ```

37. Reboot your system:
 ```
 reboot
 ```

## Conclusion

You have successfully installed Arch Linux and the KDE Plasma Desktop Environment. Enjoy your Arch Linux experience! For further customization and software installation, refer to the Arch Wiki and official documentation.



