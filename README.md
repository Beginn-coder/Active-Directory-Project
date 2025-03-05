# Active-Directory-Project

## Introduction
In the world of IT and Cybersecurity, learning Active Directory is vital for professionals and designing a homelab offers hands-on experience with essential directory services. This project allows one to enhance their skills in network administration, user management, group polices. The use of homelab provides a safe environment for simulating real-world scenarios and helps to lay a solid foundation in user authentication and access control while providing practical experience in managing network infrastructures. 

## Objectives for the project
The objective of this project is to provide hands-on experience in creating a virtualized environment for cybersecurity testing and exploitation by creating and configuring virtual machines such as Windows 10, Kali Linux, Windows Server, and Ubuntu Server. The lab aims to teach skills such as network configurations, security tool installation, endpoint monitoring, and security testing. In addition, joining windows machines to an Active Directory domain and enabling Remote Desktop. Overall, the lab enables participants to gain practical experience in cybersecurity concepts, tools, and techniques within a controlled environment.

## Tools Used
- Oracle VM VirtualBox Manager: For creating and managing virtual machines (VMs).
- Splunk Server: For log analysis and monitoring.
- Splunk Universal Forwarder: For data forwarding to Splunk.
- Sysmon: Endpoint monitoring on Windows machines.
- Crowbar: Used to simulate brute force attacks.
- Atomic Red Team (ART): Used for security testing and validation.
- PowerShell: For scripting and automation tasks.
- Microsoft Windows Event Logs: Analyzed in Splunk for security monitoring.
- Windows Server 2022: Operating system used for Active Directory Domain Services setup.
- Ubuntu Server: Used as a Splunk server in the lab setup.
- Microsoft Windows 10: Operating system for target machine in the lab.
- Kali Linux: Used as an attacker machine in the lab setup.


## Steps

### Part 1- Creating the Virtual Machines
The first thing that you will need to do is install Oracle VM VirtualBox Manager 7.0 and install it with dependencies. The other thing you’ll want to do is create a folder on your system titled: Active-Directory-Project, this is where all of your .iso files will go along with Kali Linux. 
Once done, visit https://www.microsoft.com/en-ca/software-download/windows10, get "Create Windows 10 installation media", click "Download tool now", choose "Create installation media (USB flash drive, DVD, or ISO file) for another PC", then select "ISO file" and save it to your computer. In VirtualBox, select ‘Add’ to create a new VM, give it a name and choose the downloaded Windows 10 ISO file. For the configuration, use 2 CPU’s and 409MB RAM, 50GB virtual disk and click finish. The VM will start up, follow the installation prompts, select ‘Custom: Install Windows only (advanced)’ and let Windows 10 install. 
For Kali Linux, this can be installed 2 ways. The easiest way is to download the VM version from https://www.kali.org/ along with 7Zip, extract the kali files and import it into Virtualbox. The other method is to use a kali linux .iso file and use this link: https://www.youtube.com/watch?v=MPkni85O9JA to assist with the install. 

The installation of Windows Server requires that you download the Windows Server 2022 ISO from https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022, fill out the form, download "64-bit edition", create a new VM in Oracle VM VirtualBox Manager with the ISO, 4096MB RAM, 2 CPU’s, 50GB virtual disk, and finish. Start the VM, select "Install now", choose "Windows Server 2022 Standard Evaluation (Desktop Experience)", customize settings, create a password, and finish.
To install ubuntu server, go to https://ubuntu.com/server. In products, select Ubuntu Server and download it. The version used for this lab is 22.04 LTS. Create a new VM in Oracle VM VirtualBox Manager with the ISO, 8192MB RAM, 2 CPUs, 100GB virtual disk, and finish. Start the VM, select "Try or Install Ubuntu Server", continue through a series of "Done" and "Continue", then fill out the form before continuing the installation. Finally, reboot. Error messages are expected. After rebooting, login and run sudo apt-get update && sudo apt-get upgrade -y. After this completes, hit "Enter".


### Part 2- Configuring the Networks

In VirtualBox, navigate to Tools > Network > NAT Networks > Create. Provide a name and IPv4 Prefix, for the lab, we’ll be using 192.168.10.0/24, and apply the changes. Navigate to each VM > Settings > Network, change "Attached to: NAT Network" and assign the name to the NAT Network you just created. In the Splunk VM, run sudo nano /etc/netplan/00-installer-config.yaml. The config file should be modified to look something like this:
![image](https://github.com/user-attachments/assets/ac05a95f-296d-41b1-91bf-b4c46a984ea7)

Then run sudo netplan apply to make changes. Now run ip a, you should see the IP address set to 192.168.10.10/24. To verify the connection, run ping google.com.
Now navigate to https://www.splunk.com/ and download a free trial of Splunk Enterprise for Linux (.deb). Navigate back to Splunk and run sudo apt-get install virtualbox-guest-additions-iso. Then navigate to Devices > Shared Folders> Create new Shared Folder. Navigate to the directory where you installed Splunk, check all three boxes, and continue. Reboot the virtual machine with sudo reboot. 
Run sudo apt-get install virtualbox-guest-utils then reboot once more, and then sudo adduser <your username> vboxsf. Run mkdir share to create a new directory called "share". Now run sudo mount -t vboxsf -o uid=1000,gid=1000 <directory name where .deb file is located> share/ . To verify completion, use ls -la, the ‘Share’ should be highlighted. Navigate to the share directory using cd share/ and run ls -la once more to view all the files listed in that directory. Install splunk by running sudo dpkg -i splu<hit tab to auto-complete> . You’ll then want to run cd /opt/splunk/ and run ls -la. Change into the user Splunk by running sudo -u splunk bash. Run cd bin/. Run ./start splunk, to continue press q followed by y and [ENTER]. 

To finalize this step, exit, cd bin, and finally, sudo ./splunk enable boot-start -user splunk. This will allow Splunk to start on boot as the user Splunk.

To configure the Windows Machine, in the Start Menu search for "About" > Rename this PC. Rename it to whatever you'd like, for this lab I named it ‘Target-PC’. Restart the system. Open the Command Prompt run ipconfig and view the current IPv4 Address. Navigate to the network icon at the bottom right of the window. Right click > Open Network & Internet Settings > Change adapter options > Right click the adapter > Properties > Double click on "Internet Protocol Version 4 (TCP/IPv4) Properties > Select Use the following IP address. Set IP Address to 192.168.10.100, Subnet mask to 255.255.255.0, Default gateway to 192.168.10.1, and lastly the Preferred DNS server to 8.8.8.8. Running ipconfig again should showcase the new IP address.

If your Splunk server is running, you can visit it via the browser on your target machine's browser by typing 192.168.10.10:8000 in as the URL. In the target machine visit https://www.splunk.com/ and navigate to Products > Free Trials & Downloads > Universal Forwarder > Get my free download and download the correct version for your target machine. Double-click the installed MSI file, set up basic information but don't create a password. Skip deployment server, but for Receiving Indexer set the IP/port to 192.168.10.10:9997 and install. 
Now install Sysmon by navigating to https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon. Next, navigate to https://github.com/olafhartong/sysmon-modular scroll down and select sysmonconfig.xml. Click "raw", and save the file. Extract the sysmon file, copy URL of the extracted directory, take the sysmonconfig.xml file and place it in the extracted Sysmon file.and open Powershell as administrator, and navigate to that directory. Run .\Sysmon64.exe -i ..\sysmonconfig.xml, then click agree. 
Now for the most important step, navigate to File Explorer> Local Disk (C:) > Program Files > SplunkUniversalForwarder > etc > system > local. Open Notepad as administrator and enter the following:
![image](https://github.com/user-attachments/assets/7aeee889-c11f-46c2-8e95-b184aa1411ca)

Save this file as all file types in the local folder accessed previously as "inputs.conf".

Open Services as administrator, navigate and double click SplunkForwarder, log on, and check Local System Account. Right-click Splunk Forwarder, and restart. Now, navigate to 192.168.10.10:8000 and login. Now navigate Apps > Search & Reporting and search for "index=endpoint".
If you have done everything right, when you view your Splunk Server, you should be able to see under “Selected fields” > “Host” your Target-PC and ADDC01. 

### Part 3- Configuring Active Directory

On the Windows Server, open Server Manager and select Add Roles and Features. Select Next > Next, and check Active Directory Domain Services > Add Features. Advance until you can select install.
Once you receive the message "Configuration required. Installation succeeded on ADDC01, you can advance to the next steps. Locate the flag icon at the top of the window, and select "Promote this server to a domain controller". Select "Add a new forest", because we are creating a brand-new domain. Give your domain a name, for example; ‘demo.local’. On the next page, leave all defaults and create a password. Advance until the Configuration Wizard validates prerequisites, and then hit install. This should trigger you to be signed out and have the server restart. 
When the machine is back on, you should see DEMO\Administrator as the user. In Server Manager, navigate to Tools > Active Directory Users and Computers

Right-click demodomain.local > New > Organizational Unit > Name: "IT". Right-click in the space of this new OU > New > User. First name: "Jenny", Last name: "Smith", Full name: "Jenny Smith", User logon name: "jsmith". Create a password, then finish. Create a new OU called "HR" and create another different user.
From the target machine, open Windows search and enter "Advanced System Properties" > Computer Name > Change. Check "Domain:" and enter "DEMO.LOCAL". Navigate and right-click the network icon at the bottom right of the window > Open Network & Internet Settings > Change Adapter Options > Right-click adapter > Properties > Double-click Internet Protocol Version 4 (TCP/IPv4). Change the Preferred DNS server to 192.168.10.7. In Command Prompt, run ipconfig /all to verify that DNS Servers is set to 192.168.10.7. Now navigate back to Computer Name/Domain Changes (this window should still be open), and select "OK". Login with the administrator credentials and restart the machine.

When you go to login, select "Other User", and verify the domain the login is pointing to is "DEMO". Login using the credentials of a user you created earlier under the IT or HR organizational unit.

### Part 4- Using Kali Linux Brute Force Attack

With our kali linux machine powered on, right-click the connections icon at the top right of the screen > Edit Connections > Wired connection 1 > Edit selected connection > IPv4 Settings > set Method to "Manual" > Add > Address: 192.168.10.250 > Netmask: 24 > Gateway: 192.168.10.1 > DNS servers: 8.8.8.8 > Save. Disconnect from the network, then rejoin. Open a terminal and run ip a and you should see the new ip address reflected. To verify connectivity, run ping google.com. Now run sudo apt-get update && sudo apt-get upgrade -y. 
Now in the terminal, run mkdir ad-project and sudo apt-get install -y crowbar. Once done, type in cd /usr/share/wordlists. To verify that the file “rockyou.txt.gz" is present, run ls and unzip it using sudo gunzip rockyou.txt.gz. Once done, we are going to copy the file into our ad-project directory using cp rockyou.txt ~/Desktop/ad-project. Now run cd ~/Desktop/ad-project which will change our directory into the ad-project directory. Now we will take the first 20 lines of this file and output it into a new file called passwords.txt using head -n 20 rockyou.txt > passwords.txt. 
On our target machine, search "This PC" > Properties > Advanced System Settings > Remote > check Allow remote connections to this computer > Select users > Add > Enter the usernames we created earlier such as "jsmith" and "tsmith" > Apply.
Now navigate back to the Linux machine and run crowbar -h to get more information about the Crowbar tool. Run crowbar -b rdp -u tsmith -C passwords.txt -s 192.168.10.100/32. If a password listed in the password.txt file matches a user-assigned password, a success message will appear with the password attributed to that user. 
When we switch to Splunk, in order to view all activity, run index=endpoint and UserName=tsmith. If we scroll down and select EventCode, there will be a number of events that occur but the one we want is 4625 since that event occurred over 20 times. When we look up the eventcode, 4625 means that this is 60 counts of an account failing to log on. When investigating further, it can be noted that all of the counts are occurring at approximately the same time which is an indication of brute force activity. If you also get a 4624 eventcode, this means that there’s evidence of a successful login. We can expand this event by clicking "Show all x lines", and we can then see the source of this logon.
We are going to take a step further by using Atomic Red Team to run tests on our target machine. On the win10 machine, open powershell as admin and run Set-ExecutionPolicy Bypass CurrentUser, then enter Y. Then click on Windows Security > Virus & threat protection > Manage Settings > Add or Remove Exclusions > Add an exclusion > Folder > This PC > Local Disk (C:). Now navigate back to PowerShell and run IEX ( IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing); followed by Install-AtomicRedTeam -getAtomics and Y. To run the test, we can either go to the C:) drive > AtomicRedTeam > Atomics or go to https://attack.mitre.org/ in order to view adversary attacks and techniques, as well as use it as a key for the "T" values. Run Invoke-AtomicTest T1136.001. If we see no events in splunk after running the command but we do see it after a refresh, that means that there is a gap in our protection. 

## Summary
Congratulations, you have reached the end of the walk-through! With the conclusion of the last part of this step-by-step walk-through, you should have a successful setup and communication between 4 Virtual Machines: Windows 10 Target Machine, Kali Linux Attacker Machine, Splunk, and Windows Servers. You should be able to view events with Splunk, test Splunk functionality and defense using Crowbar brute force password cracker, and more.
