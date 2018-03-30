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

*Tylko gdy mamy UEFI, powinna byc jako pierwsza
```
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

