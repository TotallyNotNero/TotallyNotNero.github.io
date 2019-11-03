---
layout: post
title: The Art of iOS BootROM Exploits
---

The iOS BootROM exploitation scene is an interesting one, mostly because it is not as active as the userland exploitation scene. There are a lot of hidden things in the BootROM (called SecureROM by Apple) that most people don't know about. In this post I will explain the iOS Bootchain, and things about the BootROM gathered from limera1n and Axi0mX's checkm8 exploit.

# Devices discussed here
T8020 - A12 Bionic
T8030 - A13 Bionic 
T8010 - A10 Fusion
T8015 - A11 Bionic
S8000/S8003 - A9
S5L8965X - iPad Air designed A7 

# The iOS Bootchain

Apple's iPhones do a lot before you get to the homescreen. Let's talk about that first before I get into the more nitty-gritty stuff.

SecureROM - The first significant code run on an iPhone. It cannot be updated after fabrication, which is what makes BootROM exploits very rare and very sought-after. It is very small - around 150,000 bytes in the A12, so it is limited in wPAC)hat it is allowed to do. It performs a task in the T8020/T8030 that are specific to the ARMv8.3-A architecture, which is initializing the PAC random seeds. It also checks if DFU (Device Firmware Upgrade) is forced, bringing us to DFU.

DFU mode - if the Bootchain cannot proceed after the BootROM and therefore can not be trusted, the phone is essentially bricked. To allow you to recover the phone, it will take you to DFU mode - since Recovery Mode is handled by iBoot, discussed later. The BootROM starts as a USB task, which gives you the Apple Mobile Device (DFU Mode) in System Report. It awaits a download request, then an iBSS image, which is subject to APTicket and signature checks. If successful, the image is booted.
