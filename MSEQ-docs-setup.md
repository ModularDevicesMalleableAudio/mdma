---
layout: page
title: MSEQ docs - setup
permalink: /documentation/MSEQ/setup
---

# MSEQ Setup

We run the MSEQ on a raspberry pi 4gb, but it should work anywhere that you have puredata and midi ports.

It is built to work with the Novation launchpad MK2, and launchcontrol XL, with 2 launchpads and one launchcontrol for each player. You don't need both players (it can be one player!) - but its definitely better with two! 

# Installation

This page details our setup, although we encourage you to take the code and use it to create your own. As such while this guid describes using the code in conjunction with Raspberry Pi and a [Blokas PiSound](https://blokas.io/) soundcard, but you could use any computer and midi interface to get midi in and out of the pi.

At Malleable, we recommend the [Blokas Patchbox OS](https://blokas.io/patchbox-os/). But it is also super fine to use rasperry pi OS. 

## Steps for Setting up a raspberry pi

- Flash SD card with latest [Blokas Patchbox OS](https://blokas.io/patchbox-os/) or [Raspbian lite image](https://www.raspberrypi.org/software/operating-systems/).
- Copy a pre-made [wpa_supplicant.conf](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md) containing your network details and a blank text file “ssh” into the boot sector of the sd card.
- Turn on, reboot, check it has wifi connection
- Do `sudo raspi-config` to change hostname & password etc
- [Install `buster-backports`](http://marksrpicluster.blogspot.com/2019/12/add-buster-backports-to-raspberry-pi.html?m=1)
- Install [pisound](https://blokas.io/pisound/docs/): `curl https://blokas.io/pisound/install.sh | sh`
- Install [zsh](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH) & [ohmyzsh](https://medium.com/wearetheledger/oh-my-zsh-made-for-cli-lovers-installation-guide-3131ca5491fb). Set up your ~/.zshrc as you like _(optional)_ 
- Reboot.
- Allow X-forwarding: `touch ~/.Xauthority`
- [Make ssh keys](https://docs.gitlab.com/ee/ssh/) and add to gitlab / github etc
- Install [`rsub`](http://blog.keyrus.co.uk/editing_files_on_a_remote_server_using_sublime.html) _(optional)_ 
- Install [`pyenv`](https://github.com/pyenv/pyenv-installer). Install python re-requisites. Install latest python 3 (or whatever python you need) _(optional)_  
- Install newer `puredata`: `sudo apt install -t buster-backports puredata`
- Install `aconnectgui`: `sudo apt-get install aconnectgui`
- Run puredata, install the dependencies using deken externals manager (see below).

## Dependencies
We've had a few. Primarily we install the latest deken externals manager then install the latest relevant versions that match your version of puredata. 

```
cyclone
ggee
iem_vanilla
iemlib
iemnet
jmmmp
la-kitchen
list-abs
maxlib
zexy
```

Make sure to add the paths to puredata when you install each external!

## MIDI setup
The sequencer currently uses 10 virtual midi ports (10 in, and 10 out). This setup reflects how we like it, but you could change it to suit your needs (this would require some editing, until we implement a midi patch bay type option). To run these virtual midi ports it is reccomended that you use `aconnect` or `aconnectgui`, or edit the script `start_MSEQ.sh`. 

`start_MSEQ.sh` determines the order of connection for the hardware in the setup when run. Because it is multiplayer, and it has then some equipment that has the same name (we have 4 launchpads, and 2 launchcontrols), then the order of connection becomes important. To get around this we have found that a USB hub with power switches such as [this](https://www.amazon.co.uk/Individual-Switches-NIAGUOJI-Portable-Expansion/dp/B08GCK9J2W/) or [this](https://www.amazon.co.uk/VEMONT-High-Speed-Aluminum-Extension-Individual-7-Ports/dp/B08NCMRBH2) work well.
