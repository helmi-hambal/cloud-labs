# Linux: Users & Groups (Quick Notes)
## Commands
adduser helmi
passwd helmi
usermod -aG sudo helmi
id helmi
groups helmi

## Files to know
/etc/passwd
/etc/shadow
/etc/group

# Linux: Users & Groups (Lab 1)

## Basic Info
whoami
id
groups

## Create User
sudo adduser testuser
id testuser
groups testuser

## Add User to Sudo
sudo usermod -aG sudo testuser
groups testuser

## Switch User
su - testuser
exit

## Remove User
sudo deluser testuser
sudo deluser --remove-home testuser
