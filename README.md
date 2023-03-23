# dotfiles
mah dotfiles


## archinstall

Enable parallel install in /etc/pacman.conf

```
rfkill unblock wifi
iwctl station wlan0 scan
iwctl station wlan0 connect <AP>
pacman -Syy
```
Do the ``archinstall`` and reboot.

Connect to network.
```
nmcli d wifi connect <AP> password <password>
```

Install the following packages:
```
sudo pacman -S nano vim firefox wget linux-headers code nvidia git ttf-jetbrains-mono-nerd kitty
```

Install yay:
```
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
makepkg -si
```

## Installing hyprland

[refer here](https://getcryst.al/site/docs/crystal-linux/nvidiawayland)

```
yay -S hyprland-nvidia-git
```

Run hyprland once to generate the default config, then add the environment variables or directly copy the ``hyprland.conf`` file
```
env = LIBVA_DRIVER_NAME,nvidia
env = XDG_SESSION_TYPE,wayland
env = GBM_BACKEND,nvidia-drm
env = __GLX_VENDOR_LIBRARY_NAME,nvidia
env = WLR_NO_HARDWARE_CURSORS,1
```

Edit ``/etc/mkinitcpio.conf`` to add following modules:
```
MODULES=(nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```

Edit ``/etc/default/grub`` and add following:
```
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet nvidia_drm.modeset=1"
```

Apply the changes using the following commands: 
```
sudo mkinitcpio -P
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

Install the following pakages: 
```
sudo pacman -S qt5-wayland qt6-wayland qt5ct libva
yay -S nvidia-vaapi-driver-git 
```
Now reboot.

Now run hyprland. The screen will be empty if you copied the config file.
Use ``SUPER + Q`` to open a terminal.


## blue
Copy the config files from ``blue/.config``.

Install the following:
```
sudo pacman -S network-manager-applet nm-connection-editor bluez bluez-utils blueman polkit-kde-agent mako wofi bottom
yay -S waybar-git hyprpaper-git wlogout
```
Enable bluetooth if its not enabled.

## greetd-tuigreet

```
yay -S greetd greetd-tuigreet
sudo systemctl enable greetd
```
Edit ``/etc/greetd/config.toml`` :
```
[default_session]
command = "tuigreet --cmd hyprland"
```
