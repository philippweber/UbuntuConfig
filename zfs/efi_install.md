# Getting EFI running on ZFS

```bash
# Root pool
sudo mkdir -p /mnt/zfs/rpool
sudo zpool import -f rpool -o readonly=on -R /mnt/zfs/rpool

# Boot pool
#sudo zpool import -f bpool -R /mnt/zfs/bpool/
sudo zpool import -f bpool -R /mnt/zfs/rpool/

# EFI partition
sudo mount /dev/sdb1 /mnt/zfs/rpool/boot/efi/

# Mount rest
for m in /dev /dev/pts /proc /sys /run; do sudo mount -B $m "/mnt/zfs/rpool${m}"; done

# Mount tmp read-write
sudo mkdir /tmp/ext_tmp
sudo mount -B /tmp/ext_tmp/ /mnt/zfs/rpool/tmp/

# Other directories needed by update-grub
mkdir -p /tmp/var_lib_os-prober/mount
sudo mount -B /tmp/var_lib_os-prober/ /mnt/zfs/rpool/var/lib/os-prober/

# Copy Grub config to remove "30_os-prober"
cp -r /mnt/zfs/rpool/etc/grub.d/ /tmp/
rm /tmp/grub.d/30_os-prober
sudo mount -B /tmp/grub.d/ /mnt/zfs/rpool/etc/grub.d/

# Set Grub timeout
cp /mnt/zfs/rpool/etc/default/grub /tmp/default-grub
sed -i 's/GRUB_TIMEOUT=.*/GRUB_TIMEOUT=30/' /tmp/default-grub
sudo mount -B /tmp/default-grub /mnt/zfs/rpool/etc/default/grub

# Enter chroot
sudo chroot /mnt/zfs/rpool/

# Recovery
sudo grub-install /dev/sdb
sudo grub-install /dev/sdb6

# Install grub
sudo apt install --reinstall grub-efi
#sudo grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=ubuntu --recheck --debug
sudo grub-install --target=x86_64-efi --efi-directory=/boot/efi --recheck --debug
sudo update-grub

# Other commands
sudo grub-probe /boot/
zfs get mountpoint bpool

# Rescuing the kernel
sudo mount /dev/sdb1 /mnt/zfs/rpool/boot/efi/

# Safely remove
for m in /dev /dev/pts /proc /sys /run /tmp /var/lib/os-prober /etc/grub.d /etc/default/grub; do sudo umount "/mnt/zfs/rpool${m}"; done
sudo umount /mnt/zfs/rpool/boot/efi/
sudo zpool export bpool
sudo zpool export rpool
```

