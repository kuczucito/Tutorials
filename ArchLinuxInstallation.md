# Poradnik instalacji najlepszej dystrybucji a r c h  l i n u x
Marzyles kiedys o posiadaniu Archa na swoim komputerze i wzbudzaniu 
zazdrosci wsrod ludzi walacych do ubuntu? Z tym poradnikiem te marzenie 
sie spelni.

# Pobieranie A r c h  L i n u x
* Torrent: https://www.archlinux.org/releng/releases/2018.03.01/torrent/
* HTTP: http://mirror.onet.pl/pub/mirrors/archlinux/iso/2018.03.01/archlnux-2018.03.01-x86_64.iso

# Sflashowanie A r c h  L i n u x na pendrive
* Windows: https://rufus.akeo.ie/downloads/rufus-2.18.exe
* *NIX: ```# dd bs=4M if=/path/to/archlinux.iso of=/dev/sdX status=progress oflag=sync```

# Lacznosc z Wi-Fi
```wifi-menu```

# Partycjonowanie dysków
* Listowanie dysków: ```fdisk -l```
* Partycjonowanie: ```cfdisk /dev/sdX```
```
*ESP size:200MB type: EFI System
root size: ile chcemy type: Linux Filesystem
```
*Tylko gdy mamy UEFI, powinna byc jako pierwsza

* Formatowanie partycji
```
mkfs.ext4 /dev/sdXY # Partycja root
mkdosfs -F 32 /dev/sdXY # Partycja ESP
```
* Montowanie partycji root
```mount /dev/sdXY (root) /mnt```

* Montownaie partycji ESP (UEFI Only)
```mkdir /mnt/boot
mount /dev/sdXY /mnt/boot
```

# Pobieranie systemu
```pacstrap /mnt base base-devel```
Opcjonalnie możemy dodac ```wpa_supplicant netctl dialog``` jesli laczymy sie z WiFi

# Generowanie fstab
```genfstab -U /mnt >> /mnt/etc/fstab```

# chroot
```arch-chroot /mnt```

# Generowanie locali
```echo "pl_PL.UTF-8 UTF-8" >> /etc/locale.gen```
```locale-gen```
```echo LANG=pl_PL.UTF-8" >> /etc/locale.conf```

# Ustawianie czasu
```ln -sf /usr/share/zoneinfo/Europe/Warsaw /etc/localtime```

# Zmiana nazwy hosta
```echo <hostname> >> /etc/hostname```

# Generowanie ramdysku
```mkinitcpio -p linux```

# Zmiana hasla dla uzytkownika root
```passwd root```

# Instalacja GRUBa
* BIOS: 
```
pacman -S grub
grub-install /dev/sdX
grub-mkconfig -o /boot/grub/grub.cfg
```

* UEFI:
```
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot
grub-mkconfig -o /boot/grub/grub.cfg
```

* System mamy juz zainstalowany, przydaloby sie go jeszcze wstepnie skonfigurowac

# Tworzenie nowego uzytkownika
```
useradd -m -G wheel -s /bin/bash <nazwa_uzytkownika>
passwd <nazwa_uzytkownika>
```

# Konfiguracja sudo
```EDITOR=nano visudo```
Usuwamy # przy ```%wheel ALL=(ALL:ALL) ALL```

# Instalacja mikrokodu (Intel only)
```
pacman -S intel-ucode
grub-mkconfig -o /boot/grub/grub.cfg
```

# Instalacja menadzera pulpitu
* LightDM (JEDYNY SLUSZNY):
```
pacman -S lightdm-gtk-greeter
systemctl enable lightdm
```
* gdm (malo rigczu):
```
pacman -S gdm
systemctl enable gdm
```

* LXDM (troche rigczu):
```
pacman -S lxdm
systemctl enable lxdm
```

# Instalacja srodowiska graficznego
* Gnome: ```pacman -S xorg-server xorg-utils xorg-xinit gnome *gnome-extra```

* Xfce: ```pacman -S xorg-server xorg-utils xorg-xinit xfce4 *xfce4-goodies```

* KDE Plasma: ```pacman -S xorg-server xorg-utils xorg-xinit plasma```

* LXDE: ```pacman -S xorg-server xorg-utils xorg-xinit lxde```

* LXqt: ```pacman -S xorg-server xorg-utils xorg-xinit lxqt breeze-icons```

* MATE: ```pacman -S xorg-server xorg-utils xorg-xinit mate *mate-extra```

* Cinnamon: ```pacman -S xorg-server xorg-utils xorg-xinit cinnamon```
```
* - opcjonalny
```


# Instalacja Network Managera
```
pacman -S networkmanager
systemctl enable NetworkManager
```
# Instalacja sterowników NVIDIA
* Zamkniete (4xx or newer)
```pacman -S nvidia-dkms```

* Zamkniete (8xxx, 9xxx 1xx-3xx)
```pacman -S nvidia-340xx-dkms```
* Starszych nie opisze, bo trzeba robic downgrade X.Orga a to ma byc poradnik dla debili, instalujcie otwarte

* Otwarte (chujowa wydajnosc)
```pacman -S xf86-video-nouveau```

# Instalacja sterownikow AMD/ATI
* Otwarte (7xxx+, R5, R7, R9, RX)
```pacman -S xf86-video-amdgpu```

* Otwarte (6xxx i starsze)
```pacman -S xf86-video-ati```

* Instalacji zamknietych nie opisze, bo trzeba downgradowac X.Orga a to ma byc poradnik dla debili, instalujcie otwarte

# Instacja sterownikow Intel
```pacman -S xf86-video-intel```

# Instalacja pacaura
```
git clone https://aur.archlinux.org/cower-git.git
cd cower-git
makepkg -si
cd ..
git clone https://aur.archlinux.org/pacaur.git
cd pacaur
makepkg -si
cd ..
rm -r cower-git
rm -r pacaur
```

# Dodanie repo multilib i wlaczenie kolorkow w pacmanie
```nano /etc/pacman.conf```
Usuwamy # przy:
```
#color
```
```
#[multilib]
#Include = /etc/pacman.d/mirrorlist
```
