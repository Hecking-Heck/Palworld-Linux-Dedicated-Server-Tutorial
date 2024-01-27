# Palworld-Linux-Dedicated-Server-Tutorial
An in-depth tutorial on how to get started with a Palworld Dedicated Server running on Debian 12 / Ubuntu 23.10

# Currently a work in progress

# Finding a Host
I chose Hetzner

I am running:
4 Cores
8GB Ram
40GB SSD

This is around 15 euro for me on Hetzner.

# Updating
sudo apt update && sudo apt upgrade

# SteamCMD Installation








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
