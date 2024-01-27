# Palworld-Linux-Dedicated-Server-Tutorial
An in-depth tutorial on how to get started with a Palworld Dedicated Server running on Debian 12 / Ubuntu 23.10

> [!IMPORTANT]
> This is a work in progress, please standby for more updates!

> [!CAUTION]
> - This script is based on Debian 12 and Ubuntu 23.10, it might work with other distros it might not

# Finding a Host
I chose Hetzner

I am running:
4 Cores
8GB Ram
40GB SSD

This is around 15 euro for me on Hetzner.

# Updating
```bash
apt update && apt dist-upgrade
```

# SteamCMD Installation
On Debian, innstall SteamCMD with all the dependencies:
```bash
apt install software-properties-common && apt-add-repository non-free && dpkg --add-architecture i386 && apt update && apt install steamcmd
```

On Ubuntu, innstall SteamCMD with all the dependencies:
```bash
apt install software-properties-common && apt-add-repository main universe restricted multiverse && dpkg --add-architecture i386 && apt update && apt install steamcmd
```

# Creation of new user (This is required)

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







# Installing and configuring UFW

First install it
sudo apt get install ufw

Now allow the correct ports
ufw allow 22
ufw allow 8211

(22 is SSH port, 8211 is Palworld Dedicated Server Port)

now enable it
ufw enable


# Cloudflare domain and DNS stuff (this is only needed if you want a custom domain for your server)
EG. play.paltopia.online:8211 instead of 123.4.5.6:8211


# Credit
This is based on the Tutorial by A1RM4X, I have tweaked and changed it to suit my purposes a bit more
[See here](https://github.com/A1RM4X/HowTo-Palworld)
