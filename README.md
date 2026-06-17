# TR-369-network-management-system#

TR‑369 Network Management System — Initial Setup
This section documents the initial environment setup for the TR‑369/USP‑based centralized network‑management system.
It covers preparing the physical server, installing VMware ESXi, and deploying an Ubuntu Server (CUI) VM that will later host the ACS and related components.

1. Server Preparation & ESXi Installation
01
Prepare the Physical Server
Ensure the hardware is ready to host ESXi and virtual machines.

Connect power, monitor, and keyboard

Verify CPU virtualization support (Intel VT‑x / AMD‑V)

Enable virtualization in BIOS

Set boot order to USB/DVD depending on installation media

02
Create ESXi Installation Media
You need a bootable installer to deploy VMware ESXi on the server.

Download ESXi ISO from VMware

Use Rufus or BalenaEtcher to create a bootable USB

Insert the USB into the server

03
Install VMware ESXi
Deploy the hypervisor that will host your Ubuntu VM.

Boot the server from the ESXi USB installer

Follow on‑screen prompts to install ESXi on the server’s storage

Set a strong root password

Reboot and remove installation media

04
Configure ESXi Management Network
Set up the network so you can access ESXi remotely.

Assign a static IP address to ESXi

Configure subnet mask and gateway

Verify network connectivity using ping

Note the ESXi management IP for browser access

05
Access ESXi Web UI
Use your computer to manage the hypervisor remotely.

Open a browser and enter: https://<ESXi-IP>

Log in using the root credentials

Confirm the dashboard loads successfully

2. Deploying Ubuntu Server (CUI) on ESXi
06
Upload Ubuntu Server ISO
Add the Ubuntu installation image to ESXi so you can create a VM.

Download Ubuntu Server ISO (22.04 LTS recommended)

In ESXi → Storage → Datastore Browser

Upload the ISO to a folder (e.g., iso/)

07
Create a New Virtual Machine
Set up the VM that will run your ACS and backend services.

Click Create/Register VM

Select Create a new virtual machine

Choose compatibility and Linux → Ubuntu (64‑bit)

Assign CPU, RAM (4–8GB recommended), and disk (40GB+)

08
Attach the Ubuntu ISO
Mount the installation media to boot the VM.

Edit VM settings

Select CD/DVD Drive → Datastore ISO file

Choose the uploaded Ubuntu ISO

Enable Connect at power on

09
Install Ubuntu Server (CUI)
Deploy the minimal command‑line Ubuntu environment.

Power on the VM and open the console

Select Install Ubuntu Server

Choose language, keyboard, and network settings

Set hostname (e.g., acs-server)

Create a user and password

Select Install OpenSSH Server (important)

Complete installation and reboot

10
Perform Initial Ubuntu Configuration
Prepare the server for ACS installation and networking.

Log in via ESXi console or SSH

Run: sudo apt update && sudo apt upgrade -y

Set a static IP if needed

Verify network connectivity with ping google.com

Confirm SSH access from your computer
