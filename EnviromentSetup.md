<Add used Kali version and Virtual box here>

**Most of the downloaded VMs are zipped with 7zip**
Tutorial used:  https://subscription.packtpub.com/video/security/9781803241920/p1/video1_3/set-up-kali-as-a-virtual-machine-virtualbox

Resource usage (requirements): 16GB+ (32GB recommended)


1. Setup Kali Linux in virtualbox
    Install virtual box (Free unlike VMWare) https://www.virtualbox.org/wiki/Downloads
    Install virtual box Kali image (Download link https://cdimage.kali.org/kali-2023.4/kali-linux-2023.4-virtualbox-amd64.7z)
        Extract with 7zip needed https://www.7-zip.org/ to folder (E.g. C:\source\VMs)
    Add the Kali image to virtualbox (Select Add (button at the right of new))
    Start kali instance, default credentials: user - Kali password - kali
    Open terminal with ctrl+alt+t
        Set keyboard langauge (Dansih in this case) : `setxkbmap -layout dk`
        update kali instance with command `sudo apt update && sudo apt upgrade`
2. Set up virtualbox Nat for networking
 _Windows firewall does not by default allow ICMP packages_
    goto _files->Tools->Network mangement_ 
    Click on the `NAT Networks` tab.
    Click on the `Create` button
    Keep the default IPv4 Prefix
    Call the network `DefaultVMNet`
    Goto the settings of the vm and click on the network settings. Click adapter 1 tab and set `Attached to` to `NAT Network`, set the `Name` to `DefaultVMNet`
2. Set up Windos virtual machine
    Download a windwos 11 developer vm from https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/ (Windows 11) (Select virtualbox)
    _The VM is using  a evaluation license, alternatively if you have access to Azure student subscription, you can get a non evalution windows version from there_
    Unzip the vm
    In virtual box, click _Files -> Import appliance _
    Select the unzipped OVA file
    Finish the import
     Goto the settings of the vm and click on the network settings. Click adapter 1 tab and set `Attached to` to `NAT Network`, set the `Name` to `DefaultVMNet`

3. Set up Metasploitable 2 vulnerable VM
    Download from https://sourceforge.net/projects/metasploitable/files/Metasploitable2/
    Unzip the file
    create new VM in Virtualbox, and choose use existing Virtual harddisk, and choose the .vmdk file unzipped
    Default credentials: user: msfadmin password:msfadmin
    Goto the settings of the vm and click on the network settings. Click adapter 1 tab and set `Attached to` to `NAT Network`, set the `Name` to `DefaultVMNet`
    Restart the VM
4. Create restore points
    click the options icon on the right of each vm in virtualbox and select snapshots, and click `Take`
