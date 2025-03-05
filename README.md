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


### Steps

## Part 1- Creating the Virtual Machines
The first thing that you will need to do is install Oracle VM VirtualBox Manager 7.0 and install it with dependencies. The other thing you’ll want to do is create a folder on your system titled: Active-Directory-Project, this is where all of your .iso files will go along with Kali Linux. 
Once done, visit https://www.microsoft.com/en-ca/software-download/windows10, get "Create Windows 10 installation media", click "Download tool now", choose "Create installation media (USB flash drive, DVD, or ISO file) for another PC", then select "ISO file" and save it to your computer. In VirtualBox, select ‘Add’ to create a new VM, give it a name and choose the downloaded Windows 10 ISO file. For the configuration, use 2 CPU’s and 409MB RAM, 50GB virtual disk and click finish. The VM will start up, follow the installation prompts, select ‘Custom: Install Windows only (advanced)’ and let Windows 10 install. 
For Kali Linux, this can be installed 2 ways. The easiest way is to download the VM version from https://www.kali.org/ along with 7Zip, extract the kali files and import it into Virtualbox. The other method is to use a kali linux .iso file and use this link: https://www.youtube.com/watch?v=MPkni85O9JA to assist with the install. 

The installation of Windows Server requires that you download the Windows Server 2022 ISO from https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022, fill out the form, download "64-bit edition", create a new VM in Oracle VM VirtualBox Manager with the ISO, 4096MB RAM, 2 CPU’s, 50GB virtual disk, and finish. Start the VM, select "Install now", choose "Windows Server 2022 Standard Evaluation (Desktop Experience)", customize settings, create a password, and finish.
To install ubuntu server, go to https://ubuntu.com/server. In products, select Ubuntu Server and download it. The version used for this lab is 22.04 LTS. Create a new VM in Oracle VM VirtualBox Manager with the ISO, 8192MB RAM, 2 CPUs, 100GB virtual disk, and finish. Start the VM, select "Try or Install Ubuntu Server", continue through a series of "Done" and "Continue", then fill out the form before continuing the installation. Finally, reboot. Error messages are expected. After rebooting, login and run sudo apt-get update && sudo apt-get upgrade -y. After this completes, hit "Enter".
