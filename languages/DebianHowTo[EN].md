###Debian How To

[Description](#description)<br>
[Introduction](#introduction)<br>
[Initial Setup](#initial-setup)<br>
[Repositories](#repositories)<br>
[Admin Rights](#admin-rights)<br>
[Hardware](#hardware)<br>
[Appearence](#appearence)<br>
[Software](#popular-software)<br>
[Final Steps](#final-steps)<br>

####Description

This guide explains how to customize a Debian testing installation for better functionality and looks, with the MATE desktop environment. This guide is based on a DVD image of Debian Jessie (development codename for Debian 8.0).

**Requirements**

- Wired internet connection
- Know how to install Debian
- Debian Jessie DVD1

**Download**

http://cdimage.debian.org/cdimage/jessie_di_beta_2/amd64/iso-dvd/<br>



####Introduction

This is how information will be presented

Example of commands you'll need to run.

```bash
#Update the package lists
nano /etc/apt/sources.list
sudo apt-get update
```

Results of commands execution. When you encounter the text user@host:~s it will let you know you are reading an example of output instead of commands to execute. The results you get may be different from the examples.

```bash
arguez@devpc:~$ echo "This is shell output"
This is shell output

```

Text blocks indicate contents of a file or text to be copied.

>&#35; Debian (testing) oficial repositories.<br>
>deb http://ftp.debian.org/debian/ testing main contrib non-free<br>
>deb http://ftp.debian.org/debian/ testing-updates main contrib non-free<br>
>deb http://security.debian.org/ testing/updates main contrib non-free<br>

*File: /etc/apt/sources.list*

**To paste commands from this text on the terminal emulator, copy the desired command and then right click on the terminal window and select "Paste".**


####Initial Setup

Open or login into a terminal window to configure the repositories and update the package list to install the necesary tools and software that will be covered in this gude.

Get your current username (will be needed later).
It's the first word before '@' when you open the terminal emulator.

```bash
arguez@devpc:~$
```
Change the current session to root user
```bash
su
```


###Repositories

Edit sources.list with nano and delete its contents, use <kbd>Ctrl</kbd> + <kbd>K</kbd>.
Copy contents from [sources.list](https://github.com/arguez/Linux-Debian-HowTo/blob/master/sources.list), right click on the console window and select "Paste".

You can find mirrors for your country on the Debian's website.

```bash
nano /etc/apt/sources.list
```

Update packages from repositories

```bash
apt-get update
```

Install repository keyrings
```bash
apt-get install linuxmint-keyring
apt-get updateprog
```


####Admin Rights

Here we will provide admin rights to a specific user and configure the system to execute tasks with elevated privileges without using the root account. Remember that the root account must be used only for administrative tasks and not for using the system on a daily basis. This step is optional but highly recomended, specially if you need to add more users capable of running commands with admin rights. 

Install sudo and gksu

```bash
apt-get install sudo gksu
```

Add your user to the sudoers group
Use the username from Initial Setup

adduser <username> sudo

```bash
adduser arguez sudo
```

Check if your username belongs to the sudo group (sudo must be listed)

```bash
groups arguez
```

Run a shell with substitute user and environment variables reset.
This will take changes into account without the need of reboot.

su <username>

```bash
su arguez
```

```bash
root@portdev:/home/arguez# su arguez
arguez@devpc:~$
```

Test admin privileges

```bash
sudo su
```

```bash
arguez@devpc:~$ sudo su

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for sysadmin:
root@portdev:/home/arguez#
```

Log out from the root account

```bash
exit
```

From now on, the root account will not be needed. Instead, the sudo command will be placed before other commands that are required to be executed with administrative privileges.


###Hardware

This sections covers how to deal with hardware issues.

**Fix USB drives mounted as read only**

In the event that you installed Debian with a LiveUSB it may happen that USB are mounted as read only.

Edit fstab and delete any entry that with "usb", if present.

```bash
sudo nano /etc/fstab
```

**Check for missing firmware**

```bash
dmesg | grep firmware
```

```bash
arguez@devpc:~$ dmesg | grep firmware
[   11.888639] iwlwifi 0000:01:00.0: firmware: failed to load iwlwifi-6000g2b-6.ucode (-2)
[   11.888643] iwlwifi 0000:01:00.0: Direct firmware load failed with error -2
[   13.182630] iwlwifi 0000:01:00.0: firmware: failed to load iwlwifi-6000g2b-5.ucode (-2)
[   13.182800] iwlwifi 0000:01:00.0: Direct firmware load failed with error -2
[   13.279988] iwlwifi 0000:01:00.0: no suitable firmware found!
[   21.877471] r8169 0000:02:00.0: firmware: failed to load rtl_nic/rtl8105e-1.fw (-2)
[   21.877632] r8169 0000:02:00.0: Direct firmware load failed with error -2
[   21.940326] r8169 0000:02:00.0 eth0: unable to load firmware patch rtl_nic/rtl8105e-1.fw (-12)
```

Find firmware packages

apt-cache <firmware name>

From the example above:

```bash
apt-cache search iwlwifi-6000g2b-6.ucode
apt-cache search iwlwifi-6000g2b-5.ucode
apt-cache search rtl_nic/rtl8105e-1.fw
```
arguez@devpc:~$ apt-cache search iwlwifi-6000g2b-6.ucode
firmware-iwlwifi - Binary firmware for Intel Wireless cards
arguez@devpc:~$ apt-cache search iwlwifi-6000g2b-5.ucode
arguez@devpc:~$ apt-cache search rtl_nic/rtl8105e-1.fw
firmware-realtek - Binary firmware for Realtek wired and wireless network adapters

Install firmware packages

```bash
sudo apt-get install firmware-iwlwifi firmware-realtek
```

If you want to install all available firmware so you aren't missing any

```bash
sudo apt-get install apt-file
sudo apt-file update
sudo apt-file --package-only search /lib/firmware/
```

```bash
arguez@devpc:~$ sudo apt-file --package-only search /lib/firmware/
amd64-microcode
atmel-firmware
bluez-firmware
dahdi-firmware-nonfree
firmware-adi
firmware-atheros
firmware-bnx2
firmware-bnx2x
firmware-brcm80211
firmware-crystalhd
firmware-intelwimax
firmware-ipw2x00
firmware-ivtv
firmware-iwlwifi
firmware-libertas
firmware-linux-free
firmware-linux-nonfree
firmware-myricom
firmware-netxen
firmware-qlogic
firmware-ralink
firmware-realtek
firmware-samsung
firmware-ti-connectivity
firmware-zd1211
intel-microcode
prism2-usb-firmware-installer
```

Install all above entries

```bash
sudo apt-get install amd64-microcode atmel-firmware bluez-firmware dahdi-firmware-nonfree firmware-adi firmware-atheros firmware-bnx2 firmware-bnx2x firmware-brcm80211 firmware-crystalhd firmware-intelwimax firmware-ipw2x00 firmware-ivtv firmware-iwlwifi firmware-libertas firmware-linux-free firmware-linux-nonfree firmware-myricom firmware-netxen firmware-qlogic firmware-ralink firmware-realtek firmware-samsung firmware-ti-connectivity firmware-zd1211 intel-microcode
```

**Enable multi-architecture**

In case you are running Debian x64:

```bash
sudo dpkg --add-architecture i386
sudo apt-get update
```

**Install sound server**

```bash
sudo apt-get install pulseaudio
```


####Appearence

Since downloading packages may take too long, at the of this section a complete setup has been included which will download and install these software packages with a single line on the terminal.

**MATE Desktop environment**

```bash
sudo apt-get install mate-desktop-environment-extras
```

MintX/MintZ themes (optional)

```bash
sudo apt-get install mint-x-icons mint-themes
```
Linux Mint Menu (optional)

```bash
sudo apt-get install mintmenu
```

**Fonts and icons**

Libre Office sifr icons

```bash
sudo apt-get install libreoffice-style-sifr
```

Gnome Font Viewer and fonts

```bash
sudo apt-get install gnome-font-viewer fonts-freefont-otf texlive-fonts-extra ttf-bitstream-vera ttf-dejavu ttf-liberation ttf-mscorefonts-installer
```

East Asian languages support

```bash
sudo apt-get install fonts-arphic-ukai fonts-arphic-uming fonts-ipafont-mincho fonts-ipafont-gothic fonts-unfonts-core
```

**Complete Setup**

All previous packages. You may also the append packages from the next section. If you decide to do so, copy the sudo apt-get install instruction just once.

```bash
sudo apt-get install mate-desktop-environment-extras mint-x-icons mint-themes mintmenu libreoffice-style-sifr gnome-font-viewer fonts-freefont-otf texlive-fonts-extra ttf-bitstream-vera ttf-dejavu ttf-liberation ttf-mscorefonts-installer fonts-arphic-ukai fonts-arphic-uming fonts-ipafont-mincho fonts-ipafont-gothic fonts-unfonts-core
```


####Popular Software

For this section, a previous knowledge with popular software packages is recommended.

If you have previus knowledge, you can modify the command line entries, removing or adding packages according to your preferences.
If in doubt, choose the minimalist setup at the end of this section which contains the packages I consider "must have".
If you like to try and learn about new software, follow the instructions as is.
Choose the complete setup if you would like to install everything at once.

**Multimedia**

Audio and video editors
```bash
sudo apt-get install audacity kdenlive
```

Audio/Video players
```bash
sudo apt-get install audacious clementine vlc
```

Graphic editors/management

```bash
sudo apt-get install cheese gimp inkscape mypaint pinta shotwell shutter
```

CD/DVD Writters

```bash
sudo apt-get install brasero k3b xfburn
```

**Office and internet**

```bash
sudo apt-get abiword calibre evince lyx okular scribus firefox thunderbird pidgin transmission
```

**User tools**

```bash
sudo apt-get install alarm-clock-applet screenruler keepassx keepass2 xdotool unison luckybackup 
```

**Development**
```bash
sudo apt-get install eclipse g++ dia-common dia-libs dia-gnome dia-rib-network dia-shapes dia2code geany git
```

Code Blocks:  http://www.codeblocks.org/downloads/26<br>
Sublime Text: http://www.sublimetext.com/3<br>

**HDD Tools (optional)**

```bash
sudo apt-get install clonezilla partclone partimage parted gparted gnome-disk-utility gsmartcontrol gddrescue testdisk
```

**System tools**

Package management
```bash
sudo apt-get install aptitude gdebi synaptic gnome-packagekit
```

Display manager
```bash
sudo apt-get install lightdm lightdm-gtk-greeter
```

Java (for end users)
```bash
apt-get install openjdk-7-jre
```

Java (for developers)
```bash
apt-get install openjdk-7-jdk
```

Java Firefox plugin
```bash
apt-get install icedtea-plugin
```

Flash Player
```bash
sudo apt-get install flashplugin-nonfree
```

Compression tools
```bash
sudo apt-get install rar unrar zip unzip unace bzip2 lzop p7zip-full p7zip-rar
```

Bluetooth manager
```bash
sudo apt-get install blueman
```

**Minimalist setup**

Install the following packages, plus all the ones listed under system tools.

```bash
sudo apt-get install gimp clementine vlc transmission evince keepassx unison  
```

**Propietary Software**

Spotify

```bash
sudo echo "# Spotify Repository" >> /etc/apt/sources.list
sudo echo "deb http://repository.spotify.com stable non-free" >> /etc/apt/sources.list
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 94558F59
sudo apt-get update
sudo apt-get install spotify-client
```

Google Chrome
http://www.google.com/chrome/

Google Earth
http://www.google.com/earth/download/ge/agree.html

Skype
http://www.skype.com/en/download-skype/skype-for-computer/

**Complete setup**

This command line block includes all packages listed from above so you don't have to wait several times for the commands to finish and grab a cup of coffee too (optional). Propietary software need to be installed manually.

```bash
sudo apt-get install audacity kdenlive audacious clementine vlc cheese gimp inkscape mypaint pinta shotwell shutter brasero k3b xfburn abiword lyx evince okular scribus firefox thunderbird pidgin transmission alarm-clock-applet keepassx keepass2 xdotool unison luckybackup eclipse g++ dia-common dia-libs dia-gnome dia-rib-network dia-shapes dia2code geany clonezilla partclone partimage parted gparted gnome-disk-utility gsmartcontrol gddrescue testdisk aptitude gdebi synaptic gnome-packagekit openjdk-7-jre icedtea-plugin flashplugin-nonfree rar unrar zip unzip unace bzip2 lzop p7zip-full p7zip-rar blueman
```

Replace openjdk-7-jre with openjdk-7-jdk if you are a developer.



####Final Steps

**Fix depedencies**

When installing software manually, you may have unmet dependencies.

```bash
sudo dpkg -i <package>
sudo apt-get -f install
sudo dpkg -i <package>
```

**Update the system**

```bash
sudo apt-get update
sudo apt-get upgrade
```
