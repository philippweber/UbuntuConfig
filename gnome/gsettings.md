# Tweaks with gsettings

```bash
# Dual monitor workspaces

gsettings list-recursively org.gnome.mutter
gsettings list-recursively org.gnome.mutter|grep only
gsettings get org.gnome.mutter workspaces-only-on-primary

gsettings set org.gnome.mutter workspaces-only-on-primary false
```

