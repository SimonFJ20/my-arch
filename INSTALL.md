
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

9. `pacstrap /mnt base linux linux-firmware`

10. `genfstab -U /mnt >> /mnt/etc/fstab`

11. `arch-chroot /mnt`

12. `ln -sf /usr/share/zoneinfo/Europe/Copenhagen`

13. `hwclock --systohc`

14. `pacman -S vim` → `y`

15. `vim /etc/locale.gen` → uncomment `en_DK.UTF-8 UTF-8`

16. `locale-gen`

17. `vim /etc/hostname` → `<hostname>`

18. `vim /etc/hosts`

    ```
    127.0.0.1    localhost
    ::1          localhost
    127.0.1.1    <hostname>.localdomain  <hostname>
    ```

19. `passwd` → `<password>` → `<password>`

20. `useradd -m <username>`

21. `passwd <username>` → `<password>` → `<password>`

22. `usermod -aG wheel,audio,video,optical,storage <username>`

23. `pacman -S sudo` → `y`

24. `EDITOR=vim visudo` → uncomment `%wheel ALL=(ALL) ALL`

25. `pacman -S grub efibootmgr dosfstools os-prober mtools` → `y`

26. `mkdir /boot/EFI`

27. `mount /dev/sda1 /boot/EFI`

28. `grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck`

29. `grub-mkconfig -o /boot/grub/grub.cfg`

30. `pacman -S networkmanager git` → `y`

31. `systemctl enable NetworkManager`

32. `exit`

33. `umount -l /mnt`

34. `reboot` or `shutdown now`

