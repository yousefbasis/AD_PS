Hi! This is a walkthrough of a Windows Server 2025 installation and configuration, Active Directory, setting up users and more. 

## Requirements 

- Windows Server 2025: https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025
- Windows 10 ISO: https://www.microsoft.com/en-us/software-download/windows10
- VirtualBox: https://www.virtualbox.org/

## Installing Windows Server 2025

### Step 1: Downloading all required files and setting up the Virtual Machine

Download all of the required files (Windows Server ISO, Windows 10 ISO, and VirtualBox)
We are going to setup Windows Server 2025 first.
- Open VirtualBox and click the "New" button at the top, next to Open, Settings, etc..
- Give your virtual machine a name, I named mine DC for Domain Controller.
- Check "Skip Unattended Installation"
- For the hardware specifications: Depending on how much RAM you have installed on your machine I would go with a minimum of 2GBs which is 2048 MBs. If you have 16GB or more go with 2-4GBs.
- I used 3-4 CPUs depending on your hardware.
- Check "Use EFI".
- For storage 50GB is enough to handle Windows Server 2025.
- Click "Finish" and "Start" the virtual machine
- Make sure you click enter as soon as the VM window pops up so it does not got into boot.

### Step 2: Installing Windows Server 2025
- Go through the installation daemon with your language and keyboard preference.
- On the "Select Image" window, select "Windows Server 2025 Evaluation (Desktop Experience)"
- Select the partition you created in the last step. 
- Install


### Step 3: Windows Server Initial Launch
- Default username will be "Administrator", set your password.
- When you see the login screen it is going to ask you to press "CTRL + ALT + DEL", on the top toolbox, click "Input" - "Keyboard" - "Insert CTRL ALT DEL"
- We need to make the VM fullscreen. Click "Devices" at the top toolbar and "Insert Guest Additions CD Image..."
- Navigate to the File Explorer inside Windows Server
- This PC
- Click on the VirtualBox Guest Additions CD Drive
- Scroll down until you find "VBoxWindowsAddition-amd64"
- Go through the installation process and reboot the VM.
- Log back in 
- Click on "View" and "Full Screen Mode"

## Setup and Static IP Configuration 

### Step 1: Setup a Host Name
- We need to setup a host name so we can uniquely identify our server.
- On the Server Manager dashboard page, you will see a menu on the left hand side, click on "Local Server"
- Click on "Computer Name"
- It will open up "System Properties" make sure you are on the "Computer Name" tab.
- Click on "Change" 
- I named my server "Server2025-DC01".
- Restart the VM.


### Step 2: Setup the Timezone
- Right click on the time and date at the bottom right.
- Click "Adjust date and time"
- Select your timezone

### Step 3: Installing Windows Updates
- Hover over the start button and right click
- Navigate to settings
- Windows Update
- Check for updates if none are available or install updates.


### Step 4: Configuring a Static IP
**Windows Servers should have a Static IP address to ensure consistent network communication as dynamic addresses from DHCP could change causing disruption to services like DNS or file sharing.** 
- Open Server Manager 
- Click on Local Server at the toolbar on the left hand side
- Click the blue link next to the Ethernet tab
- Right CLick on the Ethernet Adapter
- Click on Properties
- Select "Internet Protocol Version 4 (TCP/IPv4)
- Click on Properties
- Check "Use the following IP Address"
- Add the IP address "192.168.1.10"
- Click on Subnet Mask and it will autocomplete by itself
- Set the default gateway to "192.168.1.1"
- For Preferred DNS server set it to "127.1.1.0" if the server will act as a DNS server
- Otherwise we can use our networks primary DNS. 
**Double check our configuration**
- On CMD, enter the "ipconfig" command and the settings should be updated

## Installing Active Directory 

- In server manager, click on Manage on the right top corner
- Click "Add roles and features"
- It will open an installation wizard
- Select role based or feature based installation
- Select the server we created / renamed earlier (Server2025-DC01 for me)
- Under the Server Roles tab select on "Active Directory Domain Services"
- Click on Add Features and Next
- Confirm and Install

### Step 2: Promoting the server to the domain controller

- At the top right corner, click on the notification flag
- Click "Promote this server to a domain controller"
- Select "Add a new forest"
- Specify the root domain name to your domain name
- For Domain Controller Options, set the Directory Services Restore Mode password
- For additional options set the NetBIOS domain name to the same name as our domain we specified earlier.
- All the defaults work fine
- Click next through the rest of the options and install


### Step 3: Verifying AD is installed
- Login to "MYDOMAIN/Administrator"
- Click on Tools at the top right corner
- Ensure that "Active Directory Users and Computers", "DNS", and "Group Policy Management" is on the drop down menu.
- Open CMD, enter "nslookup (your specified domain name)" 
- If the domain name is resolved to the correct IP address we set earlier. We have successfully installed active directory!



## Active Directory Organizational Units (OUs) and Groups






