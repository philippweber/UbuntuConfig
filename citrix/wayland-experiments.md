# Trying to get keyboard grabbing working
```bash
sudo gsettings list-recursively org.gnome.mutter.wayland

sudo gsettings set org.gnome.mutter.wayland xwayland-allow-grabs true

sudo gsettings describe org.gnome.mutter.wayland xwayland-grab-access-rules

xwininfo
xprop
xprop WM_CLASS

#sudo gsettings set org.gnome.mutter.wayland xwayland-grab-access-rules "['$(sed -n 's/^Name=//p' /usr/share/applications/wfica.desktop)','$(sed -n 's/^StartupWMClass=//p' /usr/share/applications/wfica.desktop)']"
#sudo gsettings set org.gnome.mutter.wayland xwayland-grab-access-rules "['Citrix Receiver Engine','Wfica','wfica','/opt/Citrix/ICAClient/wfica']"

sudo gsettings set org.gnome.mutter.wayland xwayland-grab-access-rules "['Wfica']"

sudo gsettings set org.gnome.mutter.wayland xwayland-grab-access-rules '["Wfica", "Wfica"]'

sudo gsettings get org.gnome.desktop.input-sources xkb-options
```

