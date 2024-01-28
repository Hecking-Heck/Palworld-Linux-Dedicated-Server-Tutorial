# Palworld Linux Dedicated Server Tutorial
An in-depth tutorial on how to get started with a Palworld Dedicated Server running on Debian 12 / Ubuntu 23.10

> [!NOTE]
> For a Palworld Modding Guide, [Check Here](https://github.com/Hecking-Heck/PalworldModdingGuide)

# Important Stuff Please Read

> [!NOTE]
> This is based on the Tutorial by A1RM4X, I have tweaked and changed it to suit my purposes a bit more
[See here](https://github.com/A1RM4X/HowTo-Palworld)

> [!IMPORTANT]
> This is a work in progress, please standby for more updates!

> [!CAUTION]
> - This script is based on Debian 12 and Ubuntu 23.10, it might work with other distros it might not

# Finding a Host
I chose Hetzner, you can use [This Referral Link](https://hetzner.cloud/?ref=VdHlKzYHPKIq) if you decide to use Hetzner!

I am running:
- 4 VCores
- 8GB Ram
- 40GB SSD

This is around 15 euros for me on Hetzner and it runs perfectly fine for my 32-slot server, you may opt for less.

# Update and upgrade everything:
```bash
apt update && apt dist-upgrade
```

# SteamCMD Installation
On Debian, install SteamCMD with all the dependencies:
```bash
apt install software-properties-common && apt-add-repository non-free && dpkg --add-architecture i386 && apt update && apt install steamcmd
```

On Ubuntu, install SteamCMD with all the dependencies:
```bash
apt install software-properties-common && apt-add-repository main universe restricted multiverse && dpkg --add-architecture i386 && apt update && apt install steamcmd
```

# Creation of the Steam user (This is required)

Install sudo and create a new user steam:
```bash
apt install sudo && useradd -m steam && passwd steam
```

Log in as steam:
```bash
sudo -u steam -s
```

# Installing the Palworld Dedicated Server

Go to the steam home folder:
```bash
cd /home/steam
```

Install the Palworld dedicated server via SteamCMD:
```bash
/usr/games/steamcmd +login anonymous +app_update 2394010 validate +quit
```

Fix server log errors by creating symlinks:
```bash
cd ~/.steam && ln -s steam/steamcmd/linux32 sdk32 && ln -s steam/steamcmd/linux64 sdk64
```

Launch the server, this first run will create the settings files that we need:
```bash
cd ~/.steam/steam/steamapps/common/PalServer && ./PalServer.sh -useperfthreads -NoAsyncLoadingThread -UseMultithreadForDS
```

Copy the server settings file in the right directory then edit the settings using nano:
```bash
cp DefaultPalWorldSettings.ini Pal/Saved/Config/LinuxServer/PalWorldSettings.ini && nano Pal/Saved/Config/LinuxServer/PalWorldSettings.ini
```

## Setup a service to automate the server

Make sure all the commands below are executed as root.

### 1. Set up the maintenance script for server backups and updates.

Create the maintenance script, make it executable and give it the right user permissions:
```bash
wget https://raw.githubusercontent.com/A1RM4X/HowTo-Palworld/main/palworld-maintenance.sh -P /home/steam/ && chmod +x /home/steam/palworld-maintenance.sh && chown steam:steam /home/steam/palworld-maintenance.sh
```

Create the backup folder and give it the right permissions:
```bash
mkdir -p /home/steam/Palworld_backups && chown steam:steam /home/steam/Palworld_backups
```

Download the Palworld service file:
Credit [A1RM4X](https://github.com/A1RM4X/HowTo-Palworld) for the service file.
```bash
wget https://raw.githubusercontent.com/A1RM4X/HowTo-Palworld/main/palworld.service -P /etc/systemd/system/
```

Enable and start the service file:
```bash
systemctl enable palworld.service && systemctl daemon-reload && systemctl start palworld.service
```

# Installing and configuring UFW (Firewall)

First, install it
sudo apt get install ufw

Now allow the correct ports
```bash
ufw allow 22 && ufw allow 8211
```

> [!NOTE]
> - 22 is SSH port
> - 8211 is Palworld Dedicated Server Port

> [!CAUTION]
> - YOU MUST ENABLE PORT 22 IF YOU ARE CONNECTING VIA SSH, IF YOU DO NOT YOU WILL DISCONNECT AND NOT BE ABLE TO LOG IN AGAIN

Now enable it
```bash
ufw enable
```


# Cloudflare domain and DNS stuff
> [!NOTE]
> - This is only needed if you want a custom domain for your server
> - EG. play.paltopia.online:8211 instead of 123.4.5.6:8211

1. Get a domain, I recommend using [NameCheap](https://www.namecheap.com)
2. Connect the domain to [Cloudflare](https://www.cloudflare.com), [GUIDE](https://www.namecheap.com/support/knowledgebase/article.aspx/9607/2210/how-to-set-up-dns-records-for-your-domain-in-a-cloudflare-account/)
3. Add an A record and point it towards your server IP [GUIDE](https://developers.cloudflare.com/dns/manage-dns-records/how-to/create-dns-records/)

> [!NOTE]
> - For the A record, the Host will be the subdomain, for example, a Host of "play" will mean that "play.yourdomain.yourtld" will point to the IP address
> - If you don't want a subdomain at the start, just type "@" in the Host field. this way "yourdomain.yourtld" will point to the IP.
> - If you do not understand what I just said, the [GUIDE](https://www.namecheap.com/support/knowledgebase/article.aspx/9607/2210/how-to-set-up-dns-records-for-your-domain-in-a-cloudflare-account/) will explain it better.
When connecting in-game, use the domain name and then append ":8211" at the end.
EG. play.paltopia.net:8211

# Credit
This is based on the Tutorial by A1RM4X, I have tweaked and changed it to suit my purposes a bit more
[See here](https://github.com/A1RM4X/HowTo-Palworld)
