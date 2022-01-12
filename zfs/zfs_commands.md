# ZFS Operation Instructions

```bash
# Install ZFS
sudo apt search zfs
sudo apt install zfsutils-linux

# ZFS import (will mount)
sudo zpool import
sudo zpool import -v
sudo zpool import -f 17382993040923200061

# Import & mount on a different root
sudo mkdir /mnt/zfs/rpool
sudo zpool import -f rpool -o readonly=on -R /mnt/zfs/rpool
sudo mkdir /mnt/sticki
sudo zpool import rpool -o readonly=on -R /mnt/sticki
sudo zpool import -f rpool -o readonly=on -R /mnt/sticki

# Remove ZFS pool
sudo zpool export bpool
sudo zpool export rpool

# Status & list
sudo zpool status
sudo zpool list
sudo zpool list -v

# ZFS properties
sudo zfs get all
sudo zfs get mountpoint
sudo zfs get mounted
sudo zfs get checksum bpool
sudo zfs get checksum rpool
# Size, usage & free
sudo zfs get -H -o name,value -r used bpool
sudo zfs get -H -o name,value -r used rpool
sudo zfs get -H -o name,value -r used rpool|grep 'G$'

# IO-Stat
sudo zpool iostat
sudo zpool iostat -vl
sudo zpool iostat -vlpH
sudo zpool iostat -vlg
sudo zpool iostat -vlP

# Create a mountpoint & find filesystem
sudo mkdir -p /mnt/zfs/bpool
sudo zfs list
# Set new mountpoint & mount
sudo zfs set mountpoint=/mnt/zfs/bpool/ bpool/BOOT/ubuntu_tkai5l
sudo zfs mount bpool/BOOT/ubuntu_tkai5l
# List content
ll /mnt/zfs/bpool/
# Unmount & undo
sudo zfs umount bpool/BOOT/ubuntu_tkai5l
sudo zfs set mountpoint=/boot bpool/BOOT/ubuntu_tkai5l

# Repairing
sudo zpool scrub rpool
sudo zpool scrub bpool
sudo zpool status

# Other commands
sudo zpool history

# ZFS-Fuse is incompatible with ZFS-Utils
sudo apt-get install zfs-fuse
sudo systemctl start zfs-fuse
sudo systemctl disable zfs-fuse
sudo systemctl status zfs-fuse

# Note to myself
history|sed 's/^ *[0-9]* *//'|grep zfs.get.m >> zfs_commands.txt
history|sed 's/^ *[0-9]* *//'|grep zpool|tail -5>>zfs_commands.txt
```

