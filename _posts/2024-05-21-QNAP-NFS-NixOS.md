---
title: Setting Up NFS on a QNAP NAS for a NixOS Client
author: Wout
date: 2024-05-21 22:00:00 +0200
categories: [NixOS, Configuration]
tags: [QNAP, NAS, NFS, NixOS]
---

## Setup
* NAS: QNAP TS-231
* Firmware version : QTS 4.3.6.2665
* PC: Lenovo T590
* OS: NixOS 23.11

## Summary
Setup your QNAP NAS as a Network File System (NFS) server such that a NixOS NFS client can access a NFS share.

## Configure the server and create the share
To set up the QNAP NAS as an NFS server, several steps need to be taken. Luckily, these steps have been elaborately documented by QNAP and can be found on their [*"How to access files on NAS via NFS from UNIX/Linux clients?"*](https://www.qnap.com/en/how-to/faq/article/how-to-access-files-on-nas-via-nfs-from-unixlinux-clients) webpage.

## Configure the client
Since the client is a NixOS client, you mount the NFS share by adding the following to your `configuration.nix` file:

```nix
fileSystems."/mnt/QNAP" = {
    device = "192.168.1.x:/share/homes";
    fsType = "nfs";
  };
```

The "/mnt/QNAP" is the directory on your client where the NFS share will be mounted. The device is the IP address of the QNAP NAS followed by the shared folder, in this case, the homes folder. To ensure that the IP address of the QNAP NAS does not change after rebooting or when the DHCP lease time expires, you can use a static IP address. Finally, run the command `sudo nixos-rebuild switch`.

Aaaaaaand you’re done!

## Resources
1. How to access files on NAS via NFS from UNIX/Linux clients? [[link](https://www.qnap.com/en/how-to/faq/article/how-to-access-files-on-nas-via-nfs-from-unixlinux-clients)]
2. NixOS NFS Wiki [[link](https://nixos.wiki/wiki/NFS)]
