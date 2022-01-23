
# Install archlinux32 from scratch

`loadkeys dk-latin1`

`timedatectl set-ntp true`

Partion disk for MBR
Partition | Type | Size
---|---|---
`/dev/sda1` | `swap` | At least `+2G`
`/dev/sda2` | Bootable `linux` | all

`mkfs.ext4 /dev/sda2`

`mkswap /dev/sda1`

`mount /dev/sda2 /mnt`

`swapon /dev/sda1`

[Erich Eckner fix](https://bbs.archlinux32.org/viewtopic.php?id=3023)
`curl -Ss https://archlinux32.org/keys.php?k=5FDCA472AB93292BC678FD59255A76DB9A12601A | gpg --homedir /etc/pacman.d/gnupg --import`, sorry in advance.
You could download the long url from a shorter url using `curl https://paste.ee/d/7ejmY/0 > url.txt` and then `$(cat url.txt)`.

`pacstrap /mnt base linux linux-firmware sudo base-devel git neovim <intel|amd>-ucode grub efibootmgr dosfstools os-prober mtools networkmanager`

`genfstab -U /mnt >> /mnt/etc/fstab`

`arch-chroot /mnt`

Run Erich Eckner fix again

`ln -sf /usr/share/zoneinfo/Europe/Copenhagen`

`hwclock -w`

In `/etc/locale.gen` uncomment 2 lines matching `en_DK*`

`locale-gen`

`echo "LANG=en_DK.UTF-8" > /etc/locale.conf`

`echo "KEYMAP=dk-latin1" > /etc/vconsole.conf`

`echo "<hostname>" > /etc/hostname`

`echo "127.0.0.1    localhost" >> /etc/hosts`
`echo "::1          localhost" >> /etc/hosts`
`echo "127.0.1.1    <hostname>.localdomain  <hostname>" >> /etc/hosts`

`passwd`

`useradd -m <username>`

`passwd <username>`

`usermod -aG wheel,audio,video,optical,storage <username>`

`EDITOR=nvim visudo`

`grub-install --target=i386-pc /dev/sda`

`grub-mkconfig -o /boot/grub/grub.cfg`

`systemctl enable NetworkManager`

`exit`

`umount -l /mnt`

`reboot`

# LXDE

`sudo pacman -S xorg xorg-xinit lxde xf86-video-<amdgpu|ati|intel|nouveau|fbdev>`

`sudo systemctl enable lxdm.service`

`reboot`

