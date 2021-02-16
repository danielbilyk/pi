# Setting up Raspberry Pi from scratch

## Topics
- [Hardware](#Hardware)
- [Software Install](#Software-Install)
- [Getting Started on Pi](#Getting-Started-on-Pi)
- [Connecting to Pi](#Connecting-to-Pi)
- [Useful Tips](#Useful-Tips)

## Hardware
- Raspberry Pi
- SD card (see [SD card requirements](https://www.raspberrypi.org/documentation/installation/sd-cards.md))
- SD card reader
- Micro-USB cable
- HDMI cable
- Mouse, keyboard, display

## Software Install
- Download [Raspberry Pi OS](https://www.raspberrypi.org/software/operating-systems/)
- Install Raspberry Pi OS on SD card using [Raspberry Pi Imager](https://www.raspberrypi.org/software/)
	- Alternatively: using [Balena Etcher](https://www.balena.io/etcher/)
- Insert SD card into card slot in Raspberry Pi
- Power up Raspberry Pi
- Follow the installation process steps and have fun. [Welcome to the machine](https://open.spotify.com/track/0i8ztWwkzwBJxviDMqYdMA?si=-uIT3liFQAKB4skdJd85uQ).

## Getting Started on Pi
- Configure settings `sudo raspi-config`

**☝🏻 N.B.:** enable SSH and VNC for remote access under “Interface Options”. Don't forget to `sudo reboot` when finished.
- Generate public/private SSH key pair `ssh-keygen -t ed22519` 
- Update list of available packages and their versions, as well as install newer versions of packages `sudo apt-get update && sudo apt-get upgrade`
- Install vim `sudo apt-get install vim`

## Connecting to Pi
Check the assigned IP-address `hostname -I`

### Connect via SSH
`ssh pi@<ip_address>`
Create `authorized_keys` file under `/home/pi/.ssh` and put a public SSH key of your Mac/PC

### Connect via VNC
 `¯\_(ツ)_/¯` 

### Connect via ngrok
- Download [ngrok](https://ngrok.com/download) to Rasperry Pi
- Register on ngrok-website to get authtoken (to be found in ngrok account settings)
- Launch `screen -R <screen_name>`
- Save `authtoken`
`<path_to_.ngrok> authtoken <token>`
- Open a TCP connection on desired port
`<path_to_.ngrok> tcp <port_name>`

## Useful Stuff I Might Forget
### Using `screen`
- Exit `screen` using ctrl+A, D
- To see the list of running screens use
`screen -ls`
- Enter launched `screen -R <screen_name>`

### Assigning static IP-address to Raspberry Pi
- [Link](https://www.nextofwindows.com/how-to-configure-mi-wi-fi-as-second-router-to-extend-existing-network-same-ssid-roaming) for MiRouter
- [Link](https://www.tp-link.com/us/support/faq/187/) for TP-Link
- On Mac/Linux
`sudo vim /etc/hosts/`
Add Raspberry Pi IP-address, as well as hostname to the `/etc/hosts/` file to simplify the connection process to your Raspberry Pi on local network. Connection should be established after running
`ssh <pi_username>@<pi_hostname>`
- - - -
