# TR-369-network-management-system#



🚀 TR‑369 Network Management System
Infrastructure Setup — From Bare‑Metal Server to Ubuntu CUI VM
Before deploying the ACS, USP agents, dashboards, and automation workflows, we first build a solid foundation.
This section documents the journey from bare‑metal hardware → ESXi hypervisor → Ubuntu Server (CUI) — the core platform powering the entire TR‑369 system.

🖥️ 1. Preparing the Server & Installing ESXi
Your physical server becomes a virtualization platform using VMware ESXi.
Follow the steps below to get the hypervisor up and running.

🔧 1.1 Create a Bootable ESXi Installer
Download the ESXi ISO from VMware.

Flash it to a USB drive using Rufus or BalenaEtcher.

Insert the USB into your server.

⚙️ 1.2 Boot Into the ESXi Installer
Power on the server.

Open the boot menu.

Select the USB drive.

Wait for the ESXi installer to load.

📦 1.3 Install ESXi on the Server
Accept the license agreement.

Select the internal storage as the installation target.

Choose your keyboard layout.

Set a strong root password.

Reboot when installation completes.

🌐 1.4 Access ESXi From Your Computer
Once ESXi boots, it displays its management IP.

From your PC browser:

Code
https://<ESXi-IP-address>
Log in using the root credentials you created.

You now have a fully operational ESXi hypervisor.

🐧 2. Deploying Ubuntu Server (CUI) on ESXi
With ESXi ready, the next step is creating the VM that will host your ACS, monitoring stack, and automation services.

🆕 2.1 Create a New Virtual Machine
In the ESXi web UI:

Click Create / Register VM

Select Create a new virtual machine

Name it:
ubuntu-acs-server

Choose:

Compatibility: ESXi 7 or later

Guest OS: Linux

Version: Ubuntu Linux (64‑bit)

💾 2.2 Assign VM Resources
Recommended starting configuration:

CPU: 2 vCPUs

RAM: 4–8 GB

Storage: 40–60 GB

Network: Connect to your management VLAN or default network

You can scale resources later if needed.

📥 2.3 Mount the Ubuntu ISO
Upload the Ubuntu Server ISO to the ESXi datastore.

Attach it to the VM’s CD/DVD drive.

▶️ 2.4 Install Ubuntu Server (CUI)
Inside the VM console:

Select Ubuntu Server (no GUI)

Choose language + keyboard layout

Configure network (DHCP or static)

Set hostname (e.g., acs-server)

Create your admin user

Enable OpenSSH Server

Complete installation and reboot

🔌 2.5 SSH Into the Ubuntu VM
From your PC terminal:

Code
ssh <username>@<ubuntu-ip>
You now have full CLI access to your Ubuntu server — ready for Docker, ACS deployment, USP agents, and more.



Here is the overall gist of the entire project 

Oktopus — get the controller + MQTT broker running on your Ubuntu VM, confirm the dashboard loads
obuspa agent — spin up a second VM/container, register it to Oktopus, confirm it shows up as a managed device
LibreNMS — install (its own VM is cleanest, or same host if resources allow), add your Catalyst 2960-S and L3 switch via SNMP, confirm graphs/discovery work
daloRADIUS + FreeRADIUS — set up RADIUS auth, then point your switches' AAA config at it for admin login
Wire alerts/reporting — LibreNMS email alerts for faults, scheduled health reports
