## Setting up the storage device

- Connect the USB storage device to the Raspberry Pi
- In order to format the storage device file system as HFS+ [hfsutils](https://linux.die.net/man/1/hfsutils) & [hfsprogs](https://packages.debian.org/sid/hfsprogs) packages are needed

`sudo apt-get install hfsutils hfsprogs`
- Use `lsblk` to find NAME of the needed USB storage device

![](https://github.com/danielbilyk/pi/blob/main/images/lsblk.png)

*in my case, sda2*

- Afterwards, format the storage device to HFS+

`sudo mkfs.hfsplus /dev/<NAME> -v TimeMachine`
- Next, create a mount point on the Raspberry Pi and give it read/write permissions

`sudo mkdir /media/TimeMachine && sudo chmod -R 777 /media/TimeMachine`
- Determine the UUID of the USB device

`ls -lha /dev/disk/by-uuid`
    
![](https://github.com/danielbilyk/pi/blob/main/images/ls%20-lha.png)
- Edit [fstab](https://wiki.debian.org/fstab) to mount the USB device at [re]boot

`sudo vim /etc/fstab`

Append the following line, replacing the <UUID> with UUID determined above

`UUID=<UUID> /media/TimeMachine hfsplus force,rw,user,noauto 0 0`

![](https://github.com/danielbilyk/pi/blob/main/images/fstab.png)

- Mount the volume
`sudo mount /media/TimeMachine`
---
## Making storage device visible on the network
  
- Edit the configuration file `sudo vim /etc/netatalk/afp.conf` to mimic the behaviour of Time Capsule on the Mac *(so that it looks nice)*. Append
    
```

```

- Launch the services
```
sudo service avahi-daemon start
```
- Configure the RPi boot routine by editing crontab `crontab -e` and appending
```
@reboot sleep 30 && mount /media/TimeMachine && sleep 30 && umount /media/TimeMachine && sleep 30 && mount /media/TimeMachine && sleep 30 && service avahi-daemon start
```
---
## Connect to Pi from Mac and backup

- Connect to server: Finder —> Go —> Connect to Server…
Write `afp:<RPi_ip_address>`

![](https://github.com/danielbilyk/pi/blob/main/images/afp%20connect.png)

Login credentials will be the same as ssh credentials on the Pi
- Open the Time Machine settings and choose network TimeMachine server.
---
