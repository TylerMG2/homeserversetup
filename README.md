# My Home Server Setup
Hello, my names Tyler and this is a place where I will document my complete setup process for my home ubuntu server I am using to host all my projects like my [Custom Bingo Maker](https://bingo.tylergwin.com.au) and other in-home services such as [AdGuard](https://github.com/AdguardTeam) as a home DNS and network wide adblock. By no means will this be anywhere close to perfect or 100% secure its just a collection of what I've learnt over a couple months of diving into the deep end.

This tutorial will go over from start to finish, my home setup for hosting both my home services and my projects online. Self hosting anything online securely can be a pretty scary and difficult thing to manage and sadly theres no real way to know that your system is 100% secure. So before starting anything, I would like to ask you to assess the level of risk your willing to take and what data you could be potentially exposing to the internet. That being said there are a variety of things we can do and put in place to drastically reduce the chance and impact of an attack. From firewalls to changing default ports to setting up ssh keys all these things can make it much harder for an inexperienced or even experienced attacker to break in.

A lot of the time I will be linking to youtube videos I found useful in each section and then just discussing any variations or issues I had with the setup. In my opinion alot of these videos explain it way better than I ever could because they have alot more experience in the space then I do. I'll try my best where I see fit to describe the way I understand certain concepts but I would take these descriptions with a grain of salt as I am still very inexperiencd.

Anyway I hope you can find something useful out of this and feedback is much appreciated!

## Contents
- [My servers hardware](#my-servers-hardware)
- [Installing the Operating System](#ubuntu-server-lts-setup)
- [Setting a static IP](#setting-a-static-ip)
- [Remotely Connecting to our server](#remotely-connecting-to-our-server)
- [Initial Security Setup](#securing-your-new-server)
- [What we are going to try and build]
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

### The Root Account
During this installation you'll notice a user account is created. A great feature of Ubuntu Server in terms of security is the fact that by default, login to the root account via password is disabled. Well what does this mean exactly? Well the root account in linux is the super-user or admin of the machine, this means anything you do or any applications you run, will be run with root privileges. This isn't best practice as it increases the potentially for damage you or applications you run could have on your system. For example you wouldn't want application A accidently deleting all files used by application B because of some software bug. How do I do admin things then? This is where sudo comes in, the user account you created on setup gets added to a group known as the sudo group, this basically allows you to perform certain tasks as the root user by prefixing the command with sudo and sometimes typing a password. The first instance of this you will likely see is the commands:
```bash
sudo apt update
sudo apt upgrade
```
These commands retrieve the latest versions of any installed packages and then upgrade to those latest versions.

### What are packages?
I like to think of packages in linux like any program you install on windows, they already come with all the 'code', enviroment, dependancies (other packages for example) that it takes to run the software. To install packages you typically use a package manager (which is also a package) to automate a lot of the process. In the case of Ubuntu Server and most debian based distributions APT is the default package manager and you probably recognise it from the command above. Package managers are responsible for automating the installation, updates and removal of packages. To install of first package, run the following command.
```bash
sudo apt install neofetch
```
You'll probably be prompted for confirmation, type y. Now you'll notice a wall of text, you can ignore this most of the time. Hopefully after a few seconds the installation will be done. To test out our new package simply type the command:
```bash 
neofetch
```
If this was successful you should see a cool little text ubuntu logo and some basic system information.

## Setting a static ip

## Remotely connecting to our server
You probably don't want to constantly have your new server hooked up to some sort of monitor and have to head over to wherever it lives in your house to make changes. What if you could access the terminal of this computer from any computer in your house? This is where Secure Shell or SSH comes in. To put it simply, SSH allows for secure communication between a server (our host machine) and a client (what ever computer your connecting from). In our case we will be using SSH to remotely manage our home server. This [video](https://youtu.be/Atbl7D_yPug) gives a basic outline of how SSH secures messages sent to and from the server and client.

### How to connect
Well if you remember from the installation, we ticked a box about setting up OpenSSH, this means our system is already ready to recieve SSH connections, heres how to connect:
```bash
ssh {USER}@{HOST_IP OR HOST_NAME}
```
Here the user is the username of the account we created during the inital setup, and the host_ip/host_name is the ip of our server which we set to a static ip earlier. You should then be prompted for the password used to login to this user account. After typing this out hint enter and if everything goes to plan you should now be in the terminal of your home server. To check that its all working as expected, run the command from earlier.
```bash
neofetch
```

## Securing your new server
Security is a very complex but important part of being connected to the internet especially when it comes to self hosting. Whilst we can never truly secure our server, we can take steps to make it very difficult for bad actors to do anything harmful. This section will largely reference this [video](https://www.youtube.com/watch?v=ZhMw53Ud2tY&t=716s&ab_channel=NetworkChuck) from the 🐐 himself Network Chuck. I personally have watched a ton of his videos since becoming interested in the world of self hosting and they are incredibly enjoyable to watch as well as being very informative. I highly recommend them.

One thing I would do a bit differently from the video is only allowing access to the SSH port from ips within your local network. To do this instead of typing
```bash
sudo ufw allow {PORT}
```
use
```bash
sudo ufw allow from {DEFAULT_GATEWAY}/24 to any port {PORT}
```
Where the default gateway is typically like 192.168.1.0 and the /24 is whats known as the subnet mask (Just think of it as outlining all the possible ip addresses within your network, in this case its just 192.168.20.1 to 192.168.20.254), if you are unsure of your default gateway type the command:
```bash
ip addr
```
and look at the ip you were assigned by your ethernet connection (eno1), replace the last number with a 0. This is just another added layer of security and ensures only computers and devices on your local network can remotely access your servers terminal. In case you already added the firewall rule from the video you can delete it like so:
```bash
sudo ufw delete allow {PORT}
```

### Why does changing the default SSH port make our network more secure?
Today the internet is full of thousands upon thousands of bots searching for devices connected to the internet that can be possibly exploited. SSH access is something commonly sought after by bad actors and one way to check if a machine has SSH setup is to try and access it on the default port. By changing the default port, we may effectively hide our system from a large portion of these bots scanning the internet.

