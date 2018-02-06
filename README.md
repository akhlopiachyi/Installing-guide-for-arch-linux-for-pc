# Installing-guide-for-arch-linux-for-pc

## Installing grub on your device:

### First step:

- dhcpcd (connect to internet)
- fdisk -l (check your current partitions)
- fdisk /dev/sda (here you should define partitions for your disk)
####  For start will be great to have such partitions:
- #boot 128M              sda1
- #swap 8G                sda2
- #root at least 50G      sda3
- #home (everything else) sda4

### Second step:

- mkfs.ext2 /dev/sda1
- mkfs.ext4 /dev/sda3
- mkswap /dev/sda2
- mkfs.ext4 /dev/sda4

### Third step:

- mount /dev/sda3 /mnt
- mkdir /mnt/{boot,home}
- mount /dev/sda1 /mnt/boot
- swapon /dev/sda2
- mount /dev/sda4 /mnt/home

### Fourth step:

#### Set mirrors

- nano /etc/pacman.d/mirrorlist

Here you should find your country and move the line under the name with your country at the 
begining

#### Install base(base-devel)

- pacstrap /mnt base base-devel
- genfstab -U -p /mnt >> /mnt/etc/fstab

### Fifth step:

- arch-chroot /mnt /bin/bash
- pacman -S grub
- grub-mkconfig -o /boot/grub/grub.cfg
- grub-install --target=i386-pc --recheck /dev/sda
- exit

### Sixth step:

- umount /mnt/{boot,home,}
- reboot

## After this step you should stick the USB flash drive off the computer

## After reboot:

### You should login as 'root' and set password for root using command 'passwd'

### Run next commands for installing all necessary packages

- dhcpcd
- systemctl enable dhcpcd
- systemctl start  dhcpcd

- nano /etc/locale.gen (uncomment all neсessary languages)
- locale-gen

- ls /usr/share/zoneinfo/ (check your zoneinfo)
- timedatectl set-timezone Europe/Kiev (set your timezone)

- nano /etc/pacman.conf

### Here you should uncomment 2 lines: [multilib] and next line after it

- pacman -Syy

- useradd -m -g users -s /bin/bash <username> (create your user)
- passwd <username> (set password to your user)

- pacman -S sudo
- visudo (here you should find "root ALL=(ALL) ALL" and in the next line set your user like 
superuser by "<username> ALL=(ALL) ALL")

- pacman -S multilib-devel fakeroot

- pacman -S xorg-server xorg-xinit xorg-drivers mesa xorg-twm xorg-xclock xterm

- pacman -S xg86-video-intel libva-intel-driver

- pacman -S awesome vicious

- cp -r /etc/skel ~/.xinitrc
- echo "exec awesome" >> ~/.xinitrc
- mkdir -p ~/.config/awesome
- cp /etc/xdg/awesome/rc.lua ~/.config/awesome

- pacman -S lightdm-gtk-greeter
- systemctl enable  lightdm
- systemctl start   lightdm
