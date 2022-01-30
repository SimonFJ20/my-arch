
# Nice looking Arch + Cinnamon

## Install arch

### Connect to WiFi

If connected via. ethernet, skip this step.

1. Enter [IWCTL](https://wiki.archlinux.org/title/Iwd#iwctl): `iwtcl`
2. Find WiFi device: `device list`
3. Scan for networks: `station <device> scan`
4. List available networks: `station <device> get-networks`
5. Connect to network: `station <device> connect <network SSID>`

### Set locale for installer

1. Load danish keyboard layout: `loadkeys dk-latin1`
2. Load time `timedatectl set-ntp true`

### Format disk targetting GPT

#### Create partition table

1. List devices: `fdisk -l`
2. Enter [Fdisk](https://wiki.archlinux.org/title/Fdisk): `fdisk /dev/<device>`
3. Create `GPT` label: `g`
4. Create EFI Boot partition: `n` → `<default>` → `<default>` → `+550M`
5. Set EFI filessystem: `t` → `1` → `1`
6. Create Swap partition, (change size if needed): `n` → `<default>` → `<default>` → `+16G`
7. Set Swap filesystem: `t` → `2` → `19`
8. Create Root partition: `n` → `<default>` → `<default>` → `<default>`
9. Save new partition table: `w`

#### Format disk

1. Format EFI Boot partition to FAT32 EFI: `mkfs.fat -F32 /dev/<device>1`
2. Format Swap partition to Swap: `mkswap /dev/<device>2`
3. Format Root partition to Linux EXT4: `mkfs.ext4 /dev/<device>3`
4. Enable Swap partition: `swapon /dev/<device>2`
5. Mount Root partition: `mount /dev/<device>3 /mnt`

### Download, install and setup Arch

1. Download and install: `pacstrap /mnt base linux linux-firmware base-devel vim sudo grub efibootmgr dosfstools os-prober mtools networkmanager git`
2. Set mountables on boot: `genfstab -U >> /mnt/etc/fstab`
3. Switch operation into new system (enter chroot): `arch-chroot /mnt`
4. Set timezone: `ln -sf /usr/share/zoneinfo/Europe/Copenhagen`
5. Set hardwareclock: `hwclock -w`
6. Select locale(s) to be generated: `sed 's/# en_DK.UTF-8 UTF-8/en_DK.UTF-8 UTF-8/g' /etc/locale.gen`
7. Generate locale(s): `locale-gen`
8. Set language: `echo "LANG=en_DK.UTF-8" > /etc/locale.conf`
9. Set keyboard layout: `echo "KEYMAP=dk-latin1" > /etc/vconsole.conf`
10. Set hostname: `echo "<hostname>" > /etc/hostname`
11. Append IPv4 localhost: `echo "127.0.0.1 localhost" >> /etc/hosts`
12. Append IPv6 localhost: `echo "::1 localhost" >> /etc/hosts`
13. Append localdomain: `echo "127.0.1.1 <hostname>.localdomain <hostname>" >> /etc/hosts`
14. Start [NetworkManager](https://wiki.archlinux.org/title/NetworkManager): `systemctl enable NetworkManager`

### Setup root and user

1. Set root password: `passwd`
2. Create new user: `useradd -m <username>`
3. Set user password: `passwd <username>`
4. Set user permissions: `usermod -aG wheel,audio,video,optical,storage <username>`
5. Set `sudo` permissions: Uncomment `# %wheel ALL=(ALL) ALL` in the `/etc/sudoers` file

### Setup Boot manager

1. Create mount directory: `mkdir /boot/EFI`
2. Mount Boot partition: `mount /dev/<device>1 /boot/EFI`
3. Install [GRUB](https://wiki.archlinux.org/title/GRUB): `grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck`
4. Create boot configuration: `grub-mkconfig -o /boot/grub/grub.cfg`

### Finish up

1. Update system: `pacman -Syu`
2. Exit chroot: `exit`
3. Unmount Root partition: `umount -l /mnt`
4. Reboot system: `reboot`

## Install Cinnamon

Packages that will be installed: `xorg <video driver> cinnamon lightdm lightdm-gtk-greeter pulseaudio pulseaudio-alsa pavucontrol firefox vlc gimp xed gnome-system-monitor papirus-icon-theme arc-gtk-theme`

### Install YAY

1. Download source code: `git clone https://aur.archlinux.org/yay.git`
2. Enter folder: `cd yay`
3. Build and install: `makepkg -si`
4. Exit folder: `cd ..`
5. Delete folder: `rm -rf yay`
6. Update YAY: `yay`

### Install Xorg + Cinnamon

Skip this step if unsure

1. Select the [video driver](https://wiki.archlinux.org/title/Xorg#Driver_installation) of your liking.
2. Install [Xorg](https://wiki.archlinux.org/title/Xorg), [Cinnamon](https://wiki.archlinux.org/title/Cinnamon#Installation) and [LightDM](https://wiki.archlinux.org/title/LightDM): `sudo pacman -S xorg <video driver> cinnamon lightdm lightdm-gtk-greeter`
3. Enable startup on boot: `sudo systemctl enable lightdm`

### Install utilities

1. Install [Firefox](https://www.mozilla.org/en-US/firefox/new/), [GNOME Terminal](https://help.gnome.org/users/gnome-terminal/stable/) and some GNOME tools: `sudo pacman -S firefox gnome-terminal gnome-system-monitor`

### Install Audio

1. Install [PulseAudio](https://wiki.archlinux.org/title/PulseAudio): `sudo pacman -S pulseaudio pulseaudio-alsa pavucontrol`

### Install Themes

1. Install Cinnamon icons and theme: `sudo pacman -S papirus-icon-theme arc-gtk-theme`
2. Install GTK theme: `yay -S yaru-gtk-theme`
3. Install Cursor theme: `yay -S oreo-nord-cursors-git`
4. Configure GTK theme in `/etc/gtk-3.0/settings.ini`: *see below*
5. Set Cinnamon theme in the `Themes` application: *see below*

`/etc/gtk-3.0/settings.ini`:
```ini
[Settings]
gtk-theme-name = Yaru
gtk-icon-theme-name = Yaru
gtk-sound-theme-name = Yaru
gtk-icon-sizes = panel-menu-bar=24,24
```
![image](https://user-images.githubusercontent.com/28040410/151721342-e2172de8-5e0e-4bd3-9324-1a7b9375104f.png)

### Setup Bash

1. Make scripts folder: `mkdir .scripts -p`
2. Download `spath.py`: `wget https://gist.githubusercontent.com/SimonFJ20/a1ac9e1eaa73db95bc8fdcfca2c7f5a1/raw/6e171d89e023f7c4b60a72042c74f099f864886b/spath.py -o .scripts/spath.py`
3. Download `.bashrc`: `wget https://gist.githubusercontent.com/SimonFJ20/a0a9104a13a725ac6c835e07b9529998/raw/61259ccb5dfc800feee9fbfb85adb03ce1ef99d5/.bashrc -o .bashrc`
