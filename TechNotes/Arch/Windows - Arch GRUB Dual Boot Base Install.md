## Preinstallation
Prior to installation, make sure to disable secure boot in the BIOS in order to properly boot into the live USB Arch  environment

## Obtaining Wireless Network Connection
First open the network manager iwctl
```bash
iwctl
```
List available network devices
```bash
device list
```
Scan available networks and list them, where [device] is your device name
```bash
station [device] scan
```
```bash
station [device] get-networks
```
Connect to desired network
```bash
station [device] connect [SSID]
```

## Verify UEFI Boot Mode
Check if the following returns a filled directory
```bash
ls /sys/firmware/efi/efivars
```
## Update Date and TIme
Check datetime status
```bash
timedatectl status
```
List available timezones
```bash
timedatectl list-timezones
```
Setting the timezone
```bash
timedatectl set-timezone [timezone]
```
## Setup Keyboard Layout
List available keyboard layouts, inside you will find the various keyboard layous you would want, most common would be under `i386`
```bash
ls /usr/share/kbd/keymaps
```
Load keyboard layout (default is US key layout)
```bash
loadkeys [path to keymap]
```
## Creating Disk Partitions
List current drives and partitions
```bash
lsblk
```
List available drives with information
```bash
fdisk -l
```

Open graphical disk manager `cfdisk` to the desired install drive
```bash
cfdisk /dev/[drive name]
```
Create 2 partitions, one for `root` with type `Linux filesystem` and the other for `swap` with type `Linux swap`; allocating 4G to 8G to `swap` is usually enough, have the rest for `root`. Make sure to sort the partitions after creating them, then write the changes to disk.

## Formatting Disk Partitions
List partition names
```bash
lsblk
```
Formatting the `root` partition
```bash
mkfs.ext4 /dev/[root partition name]
```
Formatting the `swap` partition
```bash
mkswap /dev/[swap partition name]
```
## Mounting Partitions
Enable the `swap` partition
```bash
swapon /dev/[swap partition name]
```
Mount `root` partition
```bash
mount /dev/[root partition name] /mnt
```
Verify mounted partitions
```bash
lsblk
```
## Setting Fastest Download Mirrors
Create a mirrorlist backup
```bash
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
```
Update packages
```bash
pacman -Sy
```
Install pacman-contrib package (for ranking mirrors)
```bash
pacman -S pacman-contrib
```
Rank mirrors (may take a while)
```bash
rankmirrors -n 10 /etc/pacman.d/mirrorlist.bak > /etc/pacman.d/mirrorlist
```
Verify ranked mirrors
```bash
cat /etc/pacman.d/mirrorlist
```
## Installing Arch Linux
Install Arch Linux to `root`
```bash
pacstrap -i /mnt base base-devel linux linux-headers linux-firmware intel-ucode sudo nano git neovim neofetch networkmanager dhcpcd pulseaudio bluez wpa_supplicant
```
Note: for AMD CPU use `amd-ucode`
## Generating File System Table (FSTAB)
Generate FSTAB
```bash
genfstab -U /mnt >> /mnt/etc/fstab
```
Verify FSTAB
```bash
cat /mnt/etc/fstab
```
## Switch to Installed Arch Session
Change root session
```bash
arch-chroot /mnt
```
Set root password
```bash
passwd
```
Create standard user
```bash
useradd -m [username]
```
Set password for that user
```bash
passwd [username]
```
Giving user admin privileges
```bash
usermod -aG wheel,storage,power [username]
```
Enable sudo access on wheel group
```bash
EDITOR=nvim visudo
```
Uncomment the line near the bottom of the file
```
%wheel ALL=(ALL:ALL) ALL
```
## Setting System Language
Open system language file
```bash
nvim /etc/locale.gen
```
Uncomment desired language, for US english, uncomment the line
```
en_US.UTF-8 UTF-8
```
Generate the locale
```bash
locale-gen
```
Generate the locale config file and environment variable
```bash
echo LANG=en_US.UTF-8 > /etc/locale.conf
```
```bash
export LANG=en_US.UTF-8
```
## Setup Hostname and Hosts
Create the hostname file with desired hostname
```bash
echo [hostname] > /etc/hostname
```
Open host configuration
```bash
nvim /etc/hosts
```
Update hosts to have the following
```
127.0.0.1    localhost
::1          localhost
127.0.1.1    [hostname].localdomain    localhost
```
## Setup Timezones
Find desired timezone under
```
/usr/share/zoneinfo
```
Setting the timezone
```bash
ln -sf [path to timezone] /etc/localtime
```
Sync clock
```bash
hwclock --systohc
```
## Installing GRUB Bootloader
Find EFI Partition (Usually exactly 100M)
```bash
lsblk
```
Create EFI folder
```bash
mkdir /boot/efi
```
Mount EFI partition to `/boot/efi`
```
mount /dev/[EFI partition name] /boot/efi
```
Download GRUB packages
```bash
pacman -S grub efibootmgr dosfstools mtools ntfs-3g os-prober
```
Open up GRUB default config file
```bash
nvim /etc/default/grub
```
Uncomment the following line
```
GRUB_DISABLE_OS_PROBER=false
```
Install GRUB
```bash
grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck
```
Generate GRUB config file
```bash
grub-mkconfig -o /boot/grub/grub.cfg
```
## Enabling Network Services
Enabling dhcpcd service
```bash
systemctl enable dhcpcd.service
```
Enabling network manager service
```bash
systemctl enable NetworkManager.service
```
## Exiting Live USB Environment
Exit installed environment
```bash
exit
```
Unmount installed environment from live environment
```bash
umount -lR /mnt
```
Reboot and eject USB drive
```bash
reboot
```
## Remaking GRUB config
A remake of the GRUB config is required for the os prober to detect the Windows Boot
```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

## Connecting to Wireless Network (Network Manager)
Check network adapter status
```bash
nmcli dev status
```
Check if wifi is enable
```bash
nmcli radio wifi
```
If not enabled
```bash
nmcli radio wifi on
```
List available wireless networks
```bash
nmcli dev wifi list
```
Connect to wireless network
```bash
sudo nmcli --ask dev wifi connect [SSID]
```
Finally, update the system
```bash
sudo pacman -Syu
```
---
# Uninstalling Arch
## Removing Partitions
Boot back to Windows and delete the Arch occupied partitions right to left in Disk Management. Extend the Windows partition as needed.

## Removing GRUB Bootloader
Open CMD with Administrator privileges then open the disk utility
```
disk part
```
List available disks
```
disk list
```
Select disk where Arch is installed
```
select disk [arch disk number]
```
List available partitions
```
list partition
```
Select partition named  `System`
```
select partition [system partition number]
```
Mount `System` partition
```
assign letter=x
```
Exit from disk part
```
exit
```
Change directory to the `System` mounted partition
```
x:
```
```
cd EFI
```
View directories
```
dir
```
Remove GRUB
```
rd grub_uefi /s
```
Restart computer and Arch will be successfully uninstalled