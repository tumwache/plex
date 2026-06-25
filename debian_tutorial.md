# plex with linux

how to install plex media sever to access local content on your favourite screen size, lol, right?
aaanyywaayy...

## Prerequisites
should haves:
- pc with linux installed (am using debian 13)
- Optional basic skill level; basic knowledge of linux

Note:
my username is `userb`

## Step 1: Update

1. switch to super user for ease (caution needed here) on your terminal with `sudo su`
2. update sys `apt update && apt upgrade`
3. Get wgwt with `apt install wget`

## Step 2: Download plexmediaserver

1. Download from [here](https://www.plex.tv/media-server-downloads/) or with `wget https://downloads.plex.tv/plex-media-server-new/1.43.2.10687-563d026ea/debian/plexmediaserver_1.43.2.10687-563d026ea_amd64.deb`
2. Install with `dpkg -i plexmediaserver_1.43.2.10687-563d026ea_amd64.deb`

> [!NOTE]
> [This creates a plex user and plex group. Head to `sudo nano /etc/sudoers` and add plex to the `"# User privilege specification"` with `ALL=(ALL:ALL) ALL` and `apt update`]

## Step 3: Create the media plex folder

1. on the root folder (`/`) create the folder `mkdir -p /data/plex_media`
2. add user to plex group `usermod -aG plex userb` followed by `newgrp plex` to apply the group changes.
3. give the parent user (userb) rights with `chown -R plex:plex /data/plex_media` (change owner recursively on that folder and subfolders) and `chmod -R 755 /data/plex_media`

## Step 4: firewall 
if using ufw, to allow the server to stream content and talk to your devices

1. `ufw allow 32400/tcp` - to open its primary web server port
2. `ufw allow 1900/udp` - allows Plex to stream media to older smart TVs, game consoles, and legacy media players that do not have an official Plex app
3. `ufw allow 32410:32414/udp` - GDM Network Discovery on you plex client mobile app
4. `ufw allow 3005/tcp` - the port allows you to control one Plex player from another device
5. `ufw allow 8324/tcp` - roku control protocol
6. `ufw allow 21/tcp` - for ftp transfers if copying 
  
## Step 4: Start plex server
to test the effort, if you dared:

1. `systemctl enable plexmediaserver`
2. `systemctl start plexmediaserver`
3. `systemctl status plexmediaserver`
   should show `Active: active (running)` 
- open plex from web (http://yourip:32400/web/)

## finally add files to the plex folder
