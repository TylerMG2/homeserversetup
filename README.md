# My Home Server Setup
Hello, my names Tyler and this is a place where I will document my complete setup process for my home ubuntu server I am using to host all my projects like my [Custom Bingo Maker](https://bingo.tylergwin.com.au) and other in-home services such as [AdGuard](https://github.com/AdguardTeam) as a home DNS and network wide adblock. By no means will this be anywhere close to perfect or 100% secure its just a collection of what I've learnt over a couple months of diving into the deep end. Anyway I hope you can find something useful out of this and feedback is much appreciated.

This tutorial will go over from start to finish, my home setup for hosting both my home servers and my projects online. Self hosting anything online securely can be a pretty scary and difficult thing to manage and sadly theres no real way to know that your system is 100% secure. So before starting anything, I would like to ask you to assess the level of risk your willing to take and what data you could be potentially exposing to the internet. That being said there are a variety of things we can do and put in place to drastically reduce the chance of an attack. From firewalls to changing default ports to setting up ssh keys all these things can make it much harder for an inexperienced or even experienced attacker to break in.

## Contents
- [My servers hardware](#my-servers-hardware)
- [Server Operating System](#ubuntu-server-lts-setup)
- [Initial Security Setup]
- [Server Architecture Overview]
- 

### My servers hardware
I will be running my server on an old [Dell Optiplex 990 SFF](https://i.dell.com/sites/doccontent/shared-content/data-sheets/en/Documents/optiplex-990-spec-sheet.pdf), the only differences from the base is I have an extra 4GB of RAM installed totalling 12GB aswell as a 250GB SSD

### Ubuntu server LTS setup
The Linux Distribution I've chosen is the latest stable version (LTS) of Ubuntu Server found [here](https://ubuntu.com/download/server), the specific version I'm using is 22.04.3 but I'm sure most steps will still apply. The main reasons I've chosen this distribution is for it's stability, security and access to upcoming technologies. However keep in mind that starting out with this can be fairly daunting especially if you aren't very familar with bash commands or a linux operating system.
