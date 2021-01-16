# Getting Citrix running on Ubuntu 20.04
```bash
# Download packages
#https://www.citrix.com/de-de/downloads/citrix-receiver/linux/receiver-for-linux-latest.html

# Add bionic sources for libwebkitgtk, libjavascriptcoregtk,libcanberra-gtk-module
echo 'deb http://cz.archive.ubuntu.com/ubuntu bionic main universe'|sudo tee /etc/apt/sources.list.d/bionc.list
sudo vim /etc/apt/sources.list.d/bionc.list
#ll /etc/apt/sources.list.d/*
sudo apt update

# Add libwebkitgtk, libjavascriptcoregtk, libcanberra-gtk-module
sudo apt search libwebkit
sudo apt search libjavascriptcoregtk
sudo apt search canberra-gtk-module
sudo apt install libwebkitgtk-1.0-0 libjavascriptcoregtk-1.0-0 libcanberra-gtk-module

# Install Citrix ICA client
sudo dpkg -i icaclient_13.10.0.20_amd64.deb
#sudo dpkg -i icaclientWeb_13.10.0.20_amd64.deb

# Fix broken dependencies if necessary
sudo apt install -f

# List package files
dpkg-query --listfiles icaclient 
dpkg-query --listfiles icaclient |grep -i auth

# Install necessary cetificates (copy from firefox)
sudo ln -s /usr/share/ca-certificates/mozilla/* /opt/Citrix/ICAClient/keystore/cacerts

# Change log settings if necessary...
sudo vim /opt/Citrix/ICAClient/config/AuthManConfig.xml

# Launch Citrix receiver (Warning: ica files are always deleted by the receiver!)
/opt/Citrix/ICAClient/wfica -icaroot /opt/Citrix/ICAClient ~/Downloads/*-.ica 

# Your settings
ll ~/.ICAClient/
find ~/.ICAClient/

# Debug Citrix receiver using strace
strace /opt/Citrix/ICAClient/wfica -icaroot /opt/Citrix/ICAClient ~/Downloads/*-.ica |&grep ENOENT

# Debugging using debug.ini
cp /opt/Citrix/ICAClient/config/debug.ini ~/.ICAClient/
# Copy to your local directory
ln -s ~/.ICAClient/debug.ini
ll ~/.ICAClient/debug.ini debug.ini

