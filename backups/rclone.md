# Cheap backup to gdrive with rclone

```bash
# rclone backup configuration

# https://rclone.org/
# http://wiki.linuxquestions.org/wiki/Rsync_with_Google_Drive

rclone config

cat ~/.config/rclone/rclone.conf

rclone check ~/Dokumente/ 'gdrive:/Backup - Dokumente/'

# Sync dry-run (delete files on remote)
rclone sync -nv ~/Dokumente/ 'gdrive:/Backup - Dokumente/'

# Copy dry-run
rclone copy -nv ~/Dokumente/ 'gdrive:/Backup - Dokumente/'

# git only
rclone sync -v ~/Schreibtisch/git 'gdrive:/git-repo'

rclone sync -v 'gdrive:/git-repo' ~/Desktop/git

# Listing content
rclone lsd gdrive:/
rclone lsd 'gdrive:/git-repo'
rclone ls gdrive:/
rclone ls 'gdrive:/git-repo'
```

