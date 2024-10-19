# fp-pc-console
# fresh installation guide

Skip the root user at OS install

Choose manual disk install

about 500mb of free space (just a little extra space, just in case)

about 500mb of efi partition (this seems like a good minimal)

30gb root
This will contain all your system directories like /usr /var /home.  I do this to simplify backing up the entire system and retaining any software that is installed.

Slim down the Debian installation
~~~
sudo apt update
sudo apt upgrade
sudo apt remove libreoffice-*
sudo apt autoremove
~~~

~~~
sudo apt install gamemode vim flatpak
~~~

I like installing flatpaks to the home directory, so instead of using the standard url, I do this:
~~~
flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
sudo shutdown -r now (or maybe restart with xfce and save session if you need)
~~~

While installing flatpaks, if you see a red slash instead of a green checkmark, I would totally remove 
that package, and then reinstall it.  Example:
~~~
flatpak list
flatpak uninstall net.pcsx2.PCSX2
~~~

Install your software from flatpak.  Each second command is just the command on how to run it in terminal.
~~~
flatpak install flathub org.DolphinEmu.dolphin-emu --user
flatpak run org.DolphinEmu.dolphin-emu

flatpak install flathub org.libretro.RetroArch --user
flatpak run org.libretro.RetroArch

flatpak install flathub net.pcsx2.PCSX2 --user
flatpak run net.pcsx2.PCSX2

flatpak install flathub org.scummvm.ScummVM --user
flatpak run org.scummvm.ScummVM

flatpak install flathub com.github.tchx84.Flatseal --user
flatpak run com.github.tchx84.Flatseal
~~~

Use the wired 8bitDo controller that was meant for xbox or the 8bitDo xbox controllers.

Use Flatseal to allow each emulator to see the home directory by choosing "all user files" for each emulator you need.
Then add the games to a folder in the home directory.

Set up auto login with xfce:
~~~
sudo vim /etc/lightdm/lightdm.conf
~~~
uncomment this and add your user: 
~~~
autologin-user=YOUR_USER. 
~~~

Set up "pause menu" for pcsx2 for the special home button on the controller.

For Dolphin emulator, set up hotkeys for going fullscreen, saving and loading save states.
- fullscreen: Home + Y
- quit or stop: Home + A
- save slot 1: Home + L1
- save slot 2: Home + L2
- load slot 1: Home + R1
- load slot 2: Home + R2

For xfce, to auto run pcsx2, add this to the auto run in settings:

I don't usually do this anymore, but good to know.  I normally auto run the flex-launcher.

session & startup > application autostart
~~~
flatpak run net.pcsx2.PCSX2 -bigpicture -fullscreen
~~~


Add your games to:
/home/matt/Games/

Set power settings to off and set to shutdown on power press, 
in xfce Settings > Power Manager

Install Flex Launcher

You may want to skip downloading the flex-launcher with wget, and just use the one in the
home folder, if you are copying the /home directory from a backup. 

When installing the flex-launcher, ignore the warnings or errors.  It's fine.
If you are copying a home directory, and you do some of these steps, you'll erase the config.
~~~
sudo apt install wget
wget https://github.com/complexlogic/flex-launcher/releases/download/v2.1/flex-launcher_2.1_amd64.deb
sudo apt install ./flex-launcher_2.1_amd64.deb
cp -r /usr/share/flex-launcher ~/.config
sed -i "s|/usr/share/flex-launcher|$HOME/.config/flex-launcher|g" ~/.config/flex-launcher/config.ini
~~~

To edit flex-launcher menu (may not need to do this if copying existing /home directory):
~~~
vim .config/flex-launcher/config.ini
~~~

This is how you add pico to the flex-launcher(may not need to do this if copying existing /home directory):
~~~
x-terminal-emulator -e Games/pico-8/pico8 -splore
~~~

Pico wasn't autostarting, so the solution was to add terminal to the flex-launcher by editing the autostart file here.
~~~
/home/matt/.config/autostart/
~~~
OR you can just open that using files and a text editor or right click on
the icon and choose to edit "Edit Launcher".  Just click the "Run in terminal" checkbox.

The final flex launcher config should contain this:
~~~
/home/matt/.config/flex-launcher/config.ini
~~~
~~~
# Menu configurations
[Main]
Entry1=Pico8;/home/matt/.config/flex-launcher/assets/icons/plex.png;x-terminal-emulator -e /home/matt/Games/pico-8/pico8 -splore
Entry2=RetroArch;/home/matt/.config/flex-launcher/assets/icons/plex.png;flatpak run org.libretro.RetroArch -f
Entry3=PS2;/home/matt/.config/flex-launcher/assets/icons/plex.png;flatpak run net.pcsx2.PCSX2 -bigpicture -fullscreen
Entry4=ScummVM;/home/matt/.config/flex-launcher/assets/icons/plex.png;flatpak run org.scummvm.ScummVM
#Entry2=Plex;/home/matt/.config/flex-launcher/assets/icons/plex.png;/usr/share/applications/plexmediaplayer.desktop;TVF
#Entry3=Steam;/home/matt/.config/flex-launcher/assets/icons/steam.png;/usr/share/applications/steam.desktop;BigPicture
Entry5=System;/home/matt/.config/flex-launcher/assets/icons/system.png;:submenu System

[System]
Entry1=Shutdown;/home/matt/.config/flex-launcher/assets/icons/system.png;:shutdown
Entry2=Restart;/home/matt/.config/flex-launcher/assets/icons/restart.png;:restart
Entry3=Quit Launcher;/home/matt/.config/flex-launcher/assets/icons/sleep.png;:quit
~~~


You also have to enable Gamepad support in the flex-launch config file:

/home/matt/.config/flex-launcher/config.ini
~~~
[Gamepad]
Enabled=true
~~~

flex-launcher installed these too from apt install.  Just so you know.
~~~
libsdl2-image-2.0-0 libsdl2-ttf-2.0-0
flex-launcher libsdl2-image-2.0-0 libsdl2-ttf-2.0-0
~~~




This Scummvm info, wasn't needed, for some reason.  Maybe this is just raspberry pi info, so probably skip this step.
scummvm install with flatpak from flathub.org add scummvm games manually to emulation station by adding an empty file inside of each game. Example: comi.scummvm for curso of monkey island. https://www.scummvm.org/compatibility



System Rescue CD
If you are having any efi/boot/grub issues, you can use System Rescue CD/DVD/USB to fix that.  I won't go into it in detail here, since it's in another doc.
https://www.system-rescue.org/Download/
grub-install /dev/sdb

Gparted
You can fix or expand partitions after you dd them into place, with gparted

install amd drivers
https://wiki.debian.org/AtiHowTo
I never really found info on how to install Intel drivers, so I assume debian 12 just works, but if you run into issues on a fresh install, start with an intel machine first, and then do AMD.

I think I only installed the first out of all these, but here is the list anyway.
~~~
apt-get install firmware-amd-graphics libgl1-mesa-dri libglx-mesa0 mesa-vulkan-drivers xserver-xorg-video-all
~~~
