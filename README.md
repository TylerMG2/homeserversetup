# My Home Server Setup
Hello, my names Tyler and this is a place where I will document my complete setup process for my home ubuntu server I am using to host all my projects like my [Custom Bingo Maker](https://bingo.tylergwin.com.au) and other in-home services such as [AdGuard](https://github.com/AdguardTeam) as a home DNS and network wide adblock. By no means will this be anywhere close to perfect or 100% secure its just a collection of what I've learnt over a couple months of diving into the deep end.

This tutorial will go over from start to finish, my home setup for hosting both my home services and my projects online. Self hosting anything online securely can be a pretty scary and difficult thing to manage and sadly theres no real way to know that your system is 100% secure. So before starting anything, I would like to ask you to assess the level of risk your willing to take and what data you could be potentially exposing to the internet. That being said there are a variety of things we can do and put in place to drastically reduce the chance of an attack. From firewalls to changing default ports to setting up ssh keys all these things can make it much harder for an inexperienced or even experienced attacker to break in.

A lot of the time I will be linking to youtube videos I found useful in each section and then just discussing any variations or issues I had with the setup. In my opinion alot of these videos explain it way better than I ever could because they have alot more experience in the space then I do. I'll try my best where I see fit to describe the way I understand certain concepts but I would take these descriptions with a grain of salt as I am still very inexperiencd.

Anyway I hope you can find something useful out of this and feedback is much appreciated!

## Contents
- [My servers hardware](#my-servers-hardware)
- [Installing the Operating System](#ubuntu-server-lts-setup)
- [Initial Security Setup]
- [Server Architecture Overview]
- [What is cloudflare?]
- [Cloudflared tunnel setup]
- [What is Docker?]
- [Docker Networking]
- [Docker Setup]
- [What is Nginx?]
- [Basic Nginx setup]
- [Your first application]
- [Deployment Overview]
- []

## My servers hardware
I will be running my server on an old [Dell Optiplex 990 SFF](https://i.dell.com/sites/doccontent/shared-content/data-sheets/en/Documents/optiplex-990-spec-sheet.pdf), the only differences from the base is I have an extra 4GB of RAM installed totalling 12GB aswell as a 250GB SSD.

It terms of network connection, I would highly, highly recommend connecting via ethernet for an easy setup and a much more reliable and stable connection. There are options for connecting via wifi but I will not be covering that here as it is not something I'm at all familar with.

## Ubuntu server LTS setup
The Linux Distribution I've chosen is the latest stable version (LTS) of Ubuntu Server found [here](https://ubuntu.com/download/server), the specific version I'm using is 22.04.3 but I'm sure most steps will still apply to other slightly newer versions. The main reasons I've chosen this distribution is for it's stability, security and access to upcoming technologies. However keep in mind that starting out with this can be fairly daunting especially if you aren't very familar with bash commands or a linux operating system or working without a desktop enviroment (The GUI).

### Creating a bootable usb
The first steps in the installation process involve downloading the [LTS of Ubuntu Server](https://ubuntu.com/download/server) iso file and flashing it to a usb so we can later boot from it to install Ubuntu Server. It terms of software for flashing the ISO file I personally used [balenaEtcher](https://etcher.balena.io/) but I'm sure any software for flashing will do the trick. Here is a brief [tutorial](https://www.youtube.com/watch?v=c0TK0ynXLOo&ab_channel=LutetiumTech) for using balenaEtcher specifically.

### Booting from the usb
Now that you have your usb, we need to tell our computer to boot from it. To do this make sure the computer is off and then during the boot process, I find it best to spam f1, f2, f10, f12 and del (hopefully one of them works). After that you should either be in the bios or if your lucky some sort of boot selector. At this point the setup will largely depend on your motherboard, but your looking to find some sort of boot options and change the order to have the USB be the first thing your system boots from. If you need a more in depth tutorial check out [this](https://www.youtube.com/watch?v=wH9q3KSISvQ&ab_channel=AvoidErrors).

### Ubuntu server setup
Hopefully by this point you have successfully booted from the USB drive and can see some sort of installation screen. From here I would highly recommend following this [tutorial](https://www.youtube.com/watch?v=K2m52F0S2w8&ab_channel=TechHut) as it is really easy to follow and pretty much all we will need to do. The only difference is I wouldn't install the nextcloud package but its absolutely fine if you do. Optionally you could also install docker because we will be doing this in the future anyway and in general docker is a great tool.

During this installation you'll notice a user account is created. A great feature of Ubuntu Server in terms of security is the fact that by default, the root account is disabled. Well what does this mean exactly? Well the root account in linux is typically like the super user or adminstrator of the machine, this means anything you do or any applications you run, will be run at with root priveleges. This isn't best practice as it increases the potentially for damage you or applications you run could have on your system, for example you wouldn't want application A accidently deleting all files used by application B because of some software bug. How do I do admin things then??? This is where sudo comes in, the user account you created on setup gets added to a group known as the sudo group, this basically allows you to perform certain tasks as the root user by prefix the command with sudo. The first instance of this you will likely see is the commands:
```bash
sudo apt update
sudo apt upgrade
```


