# Using gdrive with rclone

```bash
# Use gdrive as a remote for git with rclone
# https://rclone.org/commands/rclone_mount/
# For rclone setup instructions see rclone.txt

# Mount gdrive using rclone
mkdir ~/Desktop/gdrive
rclone mount 'gdrive:/' ~/Desktop/gdrive/
rclone -vvvvv mount 'gdrive:/' ~/Desktop/gdrive/
rclone -v mount 'gdrive:/' ~/Desktop/gdrive/
mount|grep gdrive

# Clone a bare repository
mkdir ~/Desktop/gdrive/git-bare-repo/
git clone --bare ~/Desktop/git ~/Desktop/gdrive/git-bare-repo
ll ~/Desktop/gdrive/git-bare-repo

# Setup remote
git remote add gdrive ~/Desktop/gdrive/git-bare-repo
git remote -v

# Push & set to remote
git push --set-upstream gdrive master

# Synchronization by rclone will make access to remote impossible
# Solution: Create a subdirectory and mount it
mkdir ~/Desktop/gdrive/git-bare-repo
rclone -vvv mount 'gdrive:/git-bare-repo/' ~/Desktop/gdrive/git-bare-repo/
rclone -v mount 'gdrive:/git-bare-repo/' ~/Desktop/gdrive/git-bare-repo/

# The non bare repo
mkdir ~/Desktop/gdrive/git-repo
rclone -v mount 'gdrive:/git-repo/' ~/Desktop/gdrive/git-repo/

# Umount (choose carefully...)
fusermount -u ~/Desktop/gdrive/git-bare-repo/
fusermount -u ~/Desktop/gdrive/git-repo/
fusermount -u ~/Desktop/gdrive

# In case of connection issues, be careful...
sudo umount -f ~/Desktop/gdrive

# German version
ln -s ~/Schreibtisch/ ~/Desktop
```
