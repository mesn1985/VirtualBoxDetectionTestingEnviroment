## 1. ISO Images
1. Download the **Windows Server 2022 Trial ISO** (usable for 180 days).
2. Download the **VirtIO drivers ISO** from [Proxmox VirtIO Drivers](https://pve.proxmox.com/wiki/Windows_VirtIO_Drivers).
3. Upload both ISO files to the Proxmox ISO storage.

## 2. Create VM
This section explains how to create a VM for Windows Server 2022.  
Compared to general Linux VMs, there are a few extra steps because a driver interface called [Virtio](https://wiki.osdev.org/Virtio) is needed when hosting Windows Server 2022 on Proxmox.

1. _Create VM_ in Proxmox.
2. Configure as shown in the following subsections.

**Attention! Do not start the VM after creating it.**

#### 2.1 General
1. Set _Name_ to **Win2022**.

#### 2.2 OS
1. Set _ISO image_ to **Windows Server 2022 ISO**.
2. Set _Type_ to **Microsoft Windows**.
3. Set _Version_ to **11/2022**.

#### 2.3 System
1. Set _EFI Storage_ to the same storage used by the VM.
2. Set _TPM Storage_ to the same storage used by the VM.
3. Ensure _Qemu Agent_ is checked.
4. Set _SCSI Controller_ to **VirtIO SCSI**.

#### 2.4 Disks
1. Set _Bus/Device_ to **SCSI**.
2. Set the disk size to **50GB** (or your preferred size).

#### 2.5 CPU
1. Set _Cores_ to **2**.
2. Set _Type_ to **host**.

#### 2.6 Memory
1. Set _Memory_ to **4096MB**.

#### 2.6 Network
1. Set _Bridge_ to the bridge used for the network the host will join (e.g., default **VMBR0**).

#### 2.7 Confirm
1. Ensure that **Start after created** is unchecked, and click _Finish_.

## Prepare VM Before First Start
1. Select _Hardware_.
2. Click _Add_, and select _CD/DVD Drive_.
3. In the pop-up window, for _Storage_, select the drive where the **VirtIO** ISO is located, and choose the **VirtIO** ISO as the _ISO Image_.

## Installing Windows Server
1. Start the VM.
2. If you do not press any key fast enough during boot, you will need to restart the VM (send Ctrl+Alt+Delete).
3. When asked for a product key, choose **I don't have a product key** to use the trial version.
4. Select **Windows Server 2022 Standard (Desktop Experience)** (unless you only want to use PowerShell).
5. Choose **Custom: Install Microsoft Server Operating System only**.
6. Click **Load Driver**, and choose _Browse_. Select the _CD Drive_ with the **VirtIO** image and expand the drive.
7. Find the folder _VIOSCSI_ and expand it. Find the folder _2K22_, expand it, select the folder _amd64_, click OK, and then click _Next_.
8. Once the driver is installed, you should see the disk in the window.
9. Click _New_ to set up the disk, click _Apply_, and then _Next_.
10. Wait for the installation to finish.
11. Choose your password.
12. Send ALT+CTRL+DELETE to access the login screen.
13. Open _Device Manager_, and check if any drivers are missing (use the VirtIO drivers from the local drive).
