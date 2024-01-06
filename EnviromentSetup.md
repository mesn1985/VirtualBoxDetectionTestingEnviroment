<Add used Kali version and Virtual box here>

**Most of the downloaded VMs are zipped with 7zip**
Tutorial used:  https://subscription.packtpub.com/video/security/9781803241920/p1/video1_3/set-up-kali-as-a-virtual-machine-virtualbox

Resource usage (requirements): 16GB+ (32GB recommended)

_Beaware, unlike VMWare workstation, there is not at NAT network between host and vms (Although one could be created)_
1. Setup Kali Linux in virtualbox
    Install virtual box (Free unlike VMWare) https://www.virtualbox.org/wiki/Downloads
    Install virtual box Kali image (Download link https://cdimage.kali.org/kali-2023.4/kali-linux-2023.4-virtualbox-amd64.7z)
        Extract with 7zip needed https://www.7-zip.org/ to folder (E.g. C:\source\VMs)
    Add the Kali image to virtualbox (Select Add (button at the right of new))
    Start kali instance, default credentials: user - Kali password - kali
    Open terminal with ctrl+alt+t
        Set keyboard language (Danish in this case) : `setxkbmap -layout dk`
        update kali instance with command `sudo apt update && sudo apt upgrade`
2. Set up virtualbox Nat for networking
 _Windows firewall does not by default allow ICMP packages_
    goto _files->Tools->Network management_ 
    Click on the `NAT Networks` tab.
    Click on the `Create` button
    Keep the default IPv4 Prefix
    Call the network `DefaultVMNet`
    Goto the settings of the vm and click on the network settings. Click adapter 1 tab and set `Attached to` to `NAT Network`, set the `Name` to `DefaultVMNet`, and regenerate the Mac address
2. Set up Windows 11 virtual machine
    Download a windows 11 developer vm from https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/ (Windows 11) (Select virtualbox)
    _The VM is using  a evaluation license, alternatively if you have access to Azure student subscription, you can get a non evaluation windows version from there_
    Unzip the vm
    In virtual box, click _Files -> Import appliance _
    Select the unzipped OVA file
    Finish the import
     Goto the settings of the vm and click on the network settings. Click adapter 1 tab and set `Attached to` to `NAT Network`, set the `Name` to `DefaultVMNet`, and regenerate the Mac address

3. Set up Metasploitable 2 vulnerable VM
    Download from https://sourceforge.net/projects/metasploitable/files/Metasploitable2/
    Unzip the file
    create new VM in Virtualbox, and choose use existing Virtual harddisk, and choose the .vmdk file unzipped
    Goto the settings of the vm and click on the network settings. Click adapter 1 tab and set `Attached to` to `NAT Network`, set the `Name` to `DefaultVMNet`, and regenerate the Mac address
    Start the VM
    Default credentials: user: msfadmin password:msfadmin
    loadkeys dk (Sets for keyboard layout for the session )


4. Create restore points
    click the options icon on the right of each vm in virtualbox and select snapshots, and click `Take`


## Using static ip adresse instead of DHCP

In network manager, Set `DefaultVMNet` address/Mask(IPv4 Prefix) as 10.0.2.0/24 and Disable DHCP

|Total ip adresses|256|
|Usable ip adresses|253|
|Network|10.0.2.0|
|Gateway|10.0.2.1|
|Broadcast|10.0.2.255|
|First|10.0.2.2|
|Last|10.0.2.254|

### Kali
`$ sudo nano /etc/network/interfaces`
Add in the bottom (if eth0 is already defined, it should be removed)
```
auto eth0
iface eth0 inet static
address 10.0.2.2/24
gateway 10.0.2.1
```
`sudo systemctl restart networking`
`sudo nano /etc/resolv.conf`
Add the following line:
`nameserver 8.8.8.8`
Test with `ping www.google.com`

### Metasploitable (Ubuntu)
loadkeys dk (Sets for keyboard layout for the session )
`$ sudo nano /etc/network/interfaces`
Add in the bottom (if eth0 is already defined, it should be removed)
```
auto eth0
iface eth0 inet static
address 10.0.2.3
netmask 255.255.255.0
network 10.0.2.0
broadcast 10.0.2.255
gateway 10.0.2.1
dns-nameservers 8.8.8.8
```
`sudo /etc/init.d/networking restart`
Test with `ping www.google.com`

### Testing the network connection between metasploitable and Kali Linux
turn on both VMs
in kali execute `tcpdump icmp`
in metasploit execute ´ping 10.0.2.2´

Verify that kali linux recieves the pings

### Windows

### Ubuntu


