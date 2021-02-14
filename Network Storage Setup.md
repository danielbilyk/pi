## Setting up the storage device

- Connect the USB storage device to the Raspberry Pi
- In order to format the storage device file system as exFat, [exfat](https://github.com/relan/exfat) package is needed

`sudo apt-get update && sudo apt-get install exfat-utils`
- Use `lsblk` to find NAME of the needed USB storage device

![](https://github.com/danielbilyk/pi/blob/main/images/lsblk.png)

*in my case, sda4*

- Afterwards, format the storage device to exFat

`sudo mkfs.hfsplus /dev/<NAME> -v TimeMachine`
- Next, create a mount point on the Raspberry Pi and give it read/write permissions

`sudo mkdir /media/SSDani && sudo chmod -R 777 /media/SSDani`
- Determine the UUID of the USB device

`ls -lha /dev/disk/by-uuid`
    
![](https://github.com/danielbilyk/pi/blob/main/images/ls%20-lha.png)
- Edit [fstab](https://wiki.debian.org/fstab) to mount the USB device at [re]boot

`sudo vim /etc/fstab`

Append the following line, replacing the <UUID> with UUID determined above

`UUID=<UUID> /media/TimeMachine hfsplus force,rw,user,noauto 0 0`

![](https://github.com/danielbilyk/pi/blob/main/images/fstab.png)

- Mount the volume
`sudo mount /media/SSDani`
---
## Making storage device visible on the network

- On Pi, install [Samba](https://www.samba.org/samba/docs/SambaIntro.html)
`sudo apt install samba samba-common`
- Edit the configuration file `sudo vim /etc/samba/smb.conf`. Append

```
[MyMedia]
path = /media/YourHardDrive/
writeable = yes
create mask = 0775
directory mask = 0775
public=no
```

where [MyMedia] will be displayed as Share name on the Mac, and /media/YourHardDrive/ is the mounted location of the drive on the Pi.
- Create Samba password for the existing Pi user `sudo smbpasswd -a pi`. You will be asked for password twice.
- Restart Samba
`sudo systemctl restart smbd`
- Configure the RPi boot routine by editing crontab `crontab -e` and appending

```
@reboot sudo systemctl restart smbd.service
```
---
## Connect to the network disk from Mac

- Finder —> Go —> Connect to Server…
Write `smb:<RPi_ip_address>`

![](https://github.com/danielbilyk/pi/blob/main/images/smb%20connect.png)

Login credentials will be the same as your user's on the Pi.

---
