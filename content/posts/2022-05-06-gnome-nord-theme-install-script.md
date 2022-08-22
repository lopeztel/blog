---
title: Gnome Nord theme install script
author: lopeztel
date: 2022-05-06T20:52:46+00:00
url: /2022/05/06/gnome-nord-theme-install-script/
tags:
  - 100DaysToOffload
  - Bash
  - Gnome

---
I&#8217;ll start this post by saying that I have a thing for the [Nord theme](https://www.nordtheme.com/), every time I clean install a system I go through the trouble of applying the theme to everything, which got me thinking that I could automate most of it, at least for my gnome installs.

More info on Nord theme ports [here](https://www.nordtheme.com/ports)

My script (also available [here](https://github.com/lopeztel/bash-scripts/blob/main/install_nord_theme.sh)) should:

  * Install the gtk/kvantum themes from [https://github.com/EliverLara/Nordic](https://github.com/EliverLara/Nordic)
  * Create the nord profile for gnome-terminal from [https://github.com/arcticicestudio/nord-gnome-terminal](https://github.com/arcticicestudio/nord-gnome-terminal)
  * Add a nord theme for gedit based on [https://github.com/arcticicestudio/nord-gedit](https://github.com/arcticicestudio/nord-gedit)
  * Install the [papirus-icon-][1][theme](https://github.com/PapirusDevelopmentTeam/papirus-icon-theme) and changing icon colors to match the nord theme

The actual script as of today:

```bash
#!/bin/env bash
set -e
green="\e[0;92m"
red="\e[0;91m"
reset="\e[0m"
AUR_HELPER=paru
if [[ $(which $AUR_HELPER &>/dev/null) -eq 0 ]]; then
	echo -e "${red}-E- paru not found, using yay${reset}";
	AUR_HELPER=yay;
	echo -e "${green}-I- Done${reset}";
fi
echo -e "${green}-I- Updating with aur helper and installing dependencies${reset}"
$AUR_HELPER -Syu
$AUR_HELPER -S papirus-folders papirus-nord papirus-icon-theme chrome-gnome-shell
#gdm-tools #GDM theme is broken, don't do this one
echo -e "${green}-I- Updated with aur helper"
echo -e "-I- cloning Nordic gtk/kvantum theme${reset}"
git clone https://github.com/EliverLara/Nordic.git
sudo cp -r Nordic /usr/share/themes
rm -rf Nordic/
echo -e "${green}-I- Done, Nordic theme it should be on the list in gnome tweaks${reset}"
echo -e "${green}-I- cloning nord-gnome-terminal theme${reset}"
git clone https://github.com/arcticicestudio/nord-gnome-terminal.git
nord-gnome-terminal/src/nord.sh
rm -rf nord-gnome-terminal/
echo -e "${green}-I- Done, Nordic GNOME Terminal theme installed, it should be on the list in gnome-terminal profiles"
echo -e "-I- cloning nord-gedit theme${reset}"
git clone https://github.com/arcticicestudio/nord-gedit.git
cd nord-gedit
./install.sh
cd ../
rm -rf nord-gedit/
echo -e "${green}-I- Done, Nordic gedit theme installed, it should be on the list under Fonts & Colors tab"
echo -e "-I- Installing papirus icons${reset}"
papirus-folders -t Papirus-Dark -C nordic
echo -e "${green}-I- Done, papirus icon colors changed, it should be on the list in gnome tweaks, need to set to Papirus-Dark${reset}"
#echo -e "${green}-I- Installing Nordic gdm theme${reset}"
#set-gdm-theme set Nordic #GDM theme is broken, don't do this one
#echo -e "${green}-I- Installed Nordic gdm theme${reset}"
```

Results so far:

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/nord_theme_install-me.png#center)

Funnily enough I had to create another script to [install virt-manager](https://github.com/lopeztel/bash-scripts/blob/main/install_virt_manager.sh)  
on my computer and to test more scripts on a virtual machine:

```bash
#!/bin/env bash
set -e
green="\e[0;92m"
red="\e[0;91m"
reset="\e[0m"
# CONFIG_FILE=${CONFIG_FILE:-/etc/libvirt/libvirtd.conf}
# SOCKET_GROUP_KEY=${SOCKET_GROUP_KEY:-unix_sock_group}
# SOCKET_GROUP_VALUE=${SOCKET_GROUP_VALUE:-libvirt}
# SOCKET_RW_PERM_KEY=${SOCKET_RW_PERM_KEY:-unix_sock_rw_perms}
# SOCKET_RW_PERM_VALUE=${SOCKET_RW_PERM_VALUE:-0770}
if [[ $(/usr/bin/id -u) -ne 0 ]]; then
	echo -e "${red}-I- Not running as root, exit${reset}"
    exit
fi
echo -e "${green}-I- Installing virt-manager dependencies${reset}"
sudo pacman -S --noconfirm qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat ebtables iptables libguestfs
# sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat ebtables libguestfs
echo -e "${green}-I- Done"
echo -e "-I- Enabling and starting libvirtd.service${reset}"
sudo systemctl enable libvirtd.service
sudo systemctl start libvirtd.service
echo -e "${green}-I- Done${reset}"
systemctl status libvirtd.service
# echo -e "${green}-I- Modifying config file${reset}"
# sudo sed -n -i "s/\($SOCKET_GROUP_KEY *= *\).*/\1\"$SOCKET_GROUP_VALUE\"/" $CONFIG_FILE
# sudo sed -n -i "s/\($SOCKET_RW_PERM_KEY *= *\).*/\1\"$SOCKET_RW_PERM_VALUE\"/" $CONFIG_FILE
# echo -e "${green}-I- Done"
echo -e "-I- Modifying group permissions${reset}"
sudo usermod -a -G libvirt-qemu $(whoami)
sudo usermod -a -G libvirt $(whoami)
echo -e "${green}-I- Done"
echo -e "-I- Restarting libvirtd.service${reset}"
sudo systemctl restart libvirtd.service
echo -e "${green}-I- Done${reset}"

#taken from: https://computingforgeeks.com/install-kvm-qemu-virt-manager-arch-manjar/
```

I also have a [fork](https://github.com/lopeztel/st-sane-defaults-nord) of the [st terminal](https://st.suckless.org/) with the nord theme and some sane defaults applied

 [1]: https://github.com/PapirusDevelopmentTeam/papirus-icon-theme

---

Day 1 of my 2022&#8217;s [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)!
