
# Install Arch from scratch

## On EFI

1. `loadkeys dk-latin1`

2. `timedatectl set-ntp true`

3. `fdisk /dev/sda`

    1. `g`

    2. `n` → `<default>` → `<default>` → `+550M`

    3. `n` → `<default>` → `<default>` → `+2G`

    4. `n` → `<default>` → `<default>` → `<default>`

    5. `t` → `1` → `1`

    6. `t` → `2` → `19`

    7. `w`

4. `mkfs.fat -F32 /dev/sda1`

5. `mkswap /dev/sda2`

6. `swapon /dev/sda2`

7. `mkfs.ext4 /dev/sda3`

8. `mount /dev/sda3 /mnt`

9. `pacstrap /mnt base linux linux-firmware vim sudo grub efibootmgr dosfstools os-prober mtools networkmanager git`

10. `genfstab -U /mnt >> /mnt/etc/fstab`

11. `arch-chroot /mnt`

12. `ln -sf /usr/share/zoneinfo/Europe/Copenhagen`

13. `hwclock --systohc`

15. `vim /etc/locale.gen` → uncomment `en_DK.UTF-8 UTF-8`

16. `locale-gen`

17. `vim /etc/locale.conf` → `LANG=en_DK.UTF-8`

18. `vim /etc/vconsole.conf` → `KEYMAP=dk-latin1`

19. `vim /etc/hostname` → `<hostname>`

20. `vim /etc/hosts`

    ```
    127.0.0.1    localhost
    ::1          localhost
    127.0.1.1    <hostname>.localdomain  <hostname>
    ```

21. `passwd` → `<password>` → `<password>`

22. `useradd -m <username>`

23. `passwd <username>` → `<password>` → `<password>`

24. `usermod -aG wheel,audio,video,optical,storage <username>`

26. `EDITOR=vim visudo` → uncomment `%wheel ALL=(ALL) ALL`

28. `mkdir /boot/EFI`

29. `mount /dev/sda1 /boot/EFI`

30. `grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck`

31. `grub-mkconfig -o /boot/grub/grub.cfg`

33. `systemctl enable NetworkManager`

34. `exit`

35. `umount -l /mnt`

36. `reboot` or `shutdown now`

# Xorg, dwm, dmenu, st

## On Virtualbox VM

1. `sudo pacman -S xf86-video-fbdev xorg xorg-xinit nitrogen picom st firefox` → `<default>` → `<default> → `y
