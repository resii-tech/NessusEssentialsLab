# Nessus Essentials Lab: Vulnerability Management

## Description
Today’s lab is going to be on Vulnerability Management. We are going to be building a Vulnerability Management Lab where we install Nessus Essentials on our host machine and create a virtual environment with VMWare Workstation 17 by downloading a free copy of the Windows 10 operating system and running it inside a virtual machine (VM). We will even be installing some old, deprecated software in the Windows OS. We will conduct some vulnerability scans against that virtual machine in order to discover any vulnerabilities that might be on it. Then we are going to go ahead and remediate one or two of those so that we can observe what is happening in real time. 

<div style="page-break-after: always; visibility: hidden"></div>

The first scan will be a non-credentialed scan before any updates have been applied to the target machine. The second scan will be a credentialed scan before any updates have been applied. The third and final scan will be a credentialed scan of the target vm after updates have been applied. At the end of this lab, I will compare the results of the three scans.
<br />

## Bonus Material - Blog Post

Follow the link to read more on the strategy and various tools involved in executing passive and active reconaissance in unison to gather intel with Nessus at the forefront. There are also 5 helpful **Google Dorks**, 5 sample **Shodan** searh queries given along with a description of their purpose, and 5 different **Nmap Scripts** for different network mapping terrain:

[The Quintessential Guide to Vulnerability Management with Nessus](https://medium.com/@hispanictitanic/the-quintessential-guide-to-vulnerability-management-with-nessus-3eeaf3e885f9)

## Table of Contents

   * [Learning Objectives](#Learning-objectives)
   * [Technologies and Protocols](#Technologies-and-Protocols)
   * [Resource Links](#Resource-Links)
   * [Overview](#Overview)
   * [Continue Set Up: Download Windows 10 ISO file](#Download-Windows-10-ISO-file)
   * [Part 1: Download and Install Nessus Essentials](#Part-1-Download-and-Install-Nessus-Essentials)
   * [Part 2: Initialize Nessus Installation](#Part-2-Initialize-Nessus-Installation)
   * [Part 3: Setup Target Virtual Machine](#Part-3-Setup-Target-Virtual-Machine)
   * [Part 4: Configure Target Virtual Machine Firewall](#Part-4-Configure-Target-Virtual-Machine-Firewall)
   * [Part 5: Run Basic Network Scan (Non-Credentialed)](#Part-5-Run-Basic-Network-Scan-Non-Credentialed)
   * [Part 6: Re-Configure Target Virtual Machine](#Part-6-Re-Configure-Target-Virtual-Machine)
   * [Part 7: Run Second Scan (Credentialed)](#Part-7-Run-Second-Scan-Credentialed)
   * 
   * [Part 4: Create a New Scan in Nessus](#Part-4-Create-a-New-Scan-in-Nessus)
   * [Part 5: Run Scan 1 (Non-credentialed) and View the Results](#Part-5-Run-Scan-1-Non-credentialed-and-View-the-Results)
   * [Part 6: Run Scan 2 (Credentialed) and View the Results](#Part-6-Run-Scan-2-Credentialed-and-View-the-Results)
   * [Part 7: Update Windows Server 2022 on the Domain Controller](#Part-7-Update-Windows-Server-2022-on-the-Domain-Controller)
   * [Part 8: Run Scan 3 (Credentialed) and View the Results](#Part-8-Run-Scan-3-Credentialed-and-View-the-Results)
   * [Results](#Results)

### Learning Objectives:

1. Provisioning and deprovisioning virtual environments within VMware for Windows 10.
2. Run initial vulnerability scan without credentials against vm and observe results.
3. Run a credentialed vulnerability scan with the addition of deprecated software added in.
4. Perform andalysis and remediation of issues and run  final scan to verify remediation was successful.

### Technologies and Protocols:

* VMware Workstation 17 Player
* Nessus Essentials 
* Windows ISO + Firefox 3.6.12

> NOTE: This lab will be done on a Windows machine.

### Resource Links

I will provide the links to resources that will be required to follow along if you wish to do so: 

- [VMware Workstation Player](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html)
- [Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10)
- [Nessus Essentials](https://www.tenable.com/products/nessus/nessus-essentials)
- [Deprecated Firefox Version for VM Install](https://ftp.mozilla.org/pub/firefox/releases/3.6.12/win32/en-US/)

### Overview:

<img alt="Nessus Breakdown" src="https://github.com/resii-tech/NessusEssentialsLab/assets/129999089/c51374ec-e467-4d10-ad68-5e054bed7bf6)" width="98%" height="70%">

## Walk-Through

### Get Set Up: Download [VMware Workstaion 17 Player](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html)

### Continue Set Up: Download [Windows 10 ISO File](https://www.microsoft.com/en-us/software-download/windows10)

- Scroll down to the media creation tool and hit download now 
- Accept software license terms and agree to Windows 10 setup
- Create installation media and choose the ISO file option

![win10 download](https://github.com/resii-tech/NessusEssentialsLab/assets/129999089/b6fd88b5-ff83-425e-89bb-db1c4fa818f6)

## Part 1: Download and Install [Nessus Essentials](https://www.tenable.com/products/nessus/nessus-essentials)

- Register for an account
- You will receive an activation code in your email
- Download Nessus > Version: 10.6.2 Platform: Windows - x86_64 
- Open Nessus and install > A window should pop up with a local host URL 
- Copy/paste the URL for future use

![Download Nessus Specs](https://github.com/resii-tech/NessusEssentialsLab/assets/129999089/480b539a-62b2-440d-b8c6-22ca947ffb46)

## Part 2: Initialize Nessus Installation 

- Click Connect via SSL on web page 
- Click **Advance** on warning page > **Continue to localhost** > Nessus Essentials 
- Click skip on Get an activation code (we already have ours)
- Paste activation code from email > continue 
- Create a username and password **remember for future use**
- Click submit > and wait for download

![Nessus Login Page](https://github.com/resii-tech/NessusEssentialsLab/assets/129999089/5e5a4544-9b9b-4ee5-b89f-f6e50d2d8934)

> NOTE: Copy and save the local URL for Nessus so it is easy to access in the future.
    * **Nessus URL:**  https://localhost:8834/# 

## Part 3: Setup Target Virtual Machine

- Open up VMware workstation 
- Click Player > file > new virtual machine 
- For the Installer disc image file (ISO) browse and select your windows 10 ISO
- Customize > Memory: *4GB (4096 MB)* > Network Adapter: *Bridged*
- Click Finish > Open VM and follow the steps to install Windows 
- **Remember your user name and password** when you create an account 

![Host VM Configuration](https://github.com/resii-tech/NessusEssentialsLab/assets/129999089/e9681f3e-4144-41e9-bc92-6cd53b31f2c7)


## Part 4: Configure Target Virtual Machine Firewall

- On Target VM grab IP address > Open cmd.exe > Run  ``` >ipconfig ```
- On Host Machine ping Target VM > Open cmd.exe > Run  ``` >ping 192.168.1.132 -t ```
- Firewall settings on Target VM prevent scanning > Request Timed-Out

![Ping Before Firewall](https://github.com/resii-tech/NessusEssentialsLab/assets/129999089/e9465761-bd20-4688-b71c-7523d0ddf325)

> NOTE: We must change firewall settings on Target VM in order to scan it and run Nessus against it

- On Target VM in Search Bar > Type ```wf.msc``` > Turn off Domain, Private and Public Profiles
- Immediately the ping returns a result and we know we can proceed

![Ping After Firewall](https://github.com/resii-tech/NessusEssentialsLab/assets/129999089/567f0557-1e58-4c19-834f-e48fc5e4b3df)

## Part 5: Run Basic Network Scan (Non-Credentialed)

- My Scans > New Scan > Basic Network Scan
- Name: Windows 10 Single Host | Target: 192.168.1.132
- Hit Play to run scan

![Basic Scan Config](https://github.com/resii-tech/NessusEssentialsLab/assets/129999089/9f4ad7f2-3190-4db6-aebb-0248350a0c56)

> NOTE: Nessus is highly customizable. You can run Scheduled Scans, Change Port Settings and Create Reports as needed

- Basic Scan Results (Non-Credentiealed) shows minimal (16) vulnerabilities
- Click into each vulnerability for a Description and recommended Solutions

![Basic Scan Results](https://github.com/resii-tech/NessusEssentialsLab/assets/129999089/3eb1ccf3-e27b-4152-bdec-43362bb0f810)

## Part 6: Re-Configure Target Virtual Machine

- Enable Remote Registry > ```services.msc``` > Properties > Automatic > Start
- Enable File and Printer Sharing > ```advanced sharing``` > Turn on for Private, Guest, and All Networks
- Disable User Account Control > ```user account control``` > Lowest setting
- Registry Edit Hack (Add Local Account Token) > ```regedit.msc``` > see this [Nessus Article](https://community.tenable.com/s/article/Scanning-with-non-default-Windows-Administrator-Account?language=en_US) for Specs
- Restart Target Virtual Machine

![Regedit Hack](https://github.com/resii-tech/NessusEssentialsLab/assets/129999089/9080fa7c-14cd-4a49-9458-bfaf5a41a8fd)

## Part 7: Run Second Scan (Credentialed)

- In Nessus > Check box > More > Configure > Credentials Tab
- Windows > Enter Username: ```randall``` | Password: ```########```
> NOTE: To find Username > Open ```cmd.exe``` > Type ```>whoami```
- Save and Run Scan



- Second Scan Results (Credentialed) shows a significant increase (#) in vulnerabilities
- This highlights the importance of running a ***Credentialed Scan*** and the info it provides

## 











## Results

### Scan 1 (Non-credentialed) Results
| Risk Raiting | Count | Percentage |
| --- | --- | --- |
| Critical | 0 | 0.00% |
| High | 0 | 0.00% |
| Medium | 1 | 1.45% |
| Low | 1 | 1.45% |
| Info | 67 | 97.10% |
| **Total** | **69** | **100.00%** |

The first scan was a non-credentialed scan before any updates were applied to the domain controller. This scan produced 1 medium, 1 low, and 67 info results. Non-credentialed scans are not as useful as credentialed scans when trying to get an accurate view of vulnerabilities that exist on a device or network because they lack proper access. Non-credentialed scans cannot see vulnerabilities that may exist on parts of a device or network that are off limits without proper authentication. As a result, non-credentialed scans produce far less results than credentialed scans.

### Scan 2 (Credentialed) Results
| Risk Raiting | Count | Percentage |
| --- | --- | --- |
| Critical | 10 | 3.79% |
| High | 7 | 2.65% |
| Medium | 6 | 2.27% |
| Low | 1 | 0.38% |
| Info | 240 | 90.91% |
| **Total** | **264** | **100.00%** |

The second scan was a credentialed scan before any updates were applied to the domain controller. This scan produced 10 critical, 7 high, 6 medium, 1 low, and 240 info results. Since this scan was configured with the proper credentials, a lot more results were produced.

### Scan 3 (Credentialed) Results
| Risk Raiting | Count | Percentage |
| --- | --- | --- |
| Critical | 0 | 0.00% |
| High | 0 | 0.00% |
| Medium | 1 | 1.09% |
| Low | 1 | 1.09% |
| Info | 90 | 97.83% |
| **Total** | **92** | **100.00%** |

The third scan was a credentialed scan after updates had been applied to the domain controller. The results of this scan showed far less vulnerabilities than the previous credentialed scan I ran before updates were applied. This scan produced 1 medium, 1 low, and 90 info results. All of the critical and high level vulnerabilities were fixed. These results demostrate the importance of schedueling regular vulnerability scan agaisnt networks and devices. They also show that keeping systems up to date with the latest patches can drastically reduce the number of vulnerabilities an attacker could exploit.

#### False Positive Result
The remaining medium rank vulnerability appears to be a false positive. 

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/1e3f52e4-7ea2-4b4d-af2c-c57890a590f2" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/16055178-dfae-4d25-ba72-5f27c5b9a263" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/e2f6d5fc-4ef6-4bc3-afa7-a5da638c12df" height="80%" width="80%"/>
</br>
</br>
