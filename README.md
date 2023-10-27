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

<img width="961" alt="nessus breakdown" src="https://github.com/resii-tech/NessusEssentialsLab/assets/129999089/c51374ec-e467-4d10-ad68-5e054bed7bf6">

## Walk-Through

### Get Set Up: Download [VMware Workstaion 17 Player](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html)

### Continue Set Up: Download [Windows 10 ISO File](https://www.microsoft.com/en-us/software-download/windows10)
- Scroll down to the media creation tool and hit download now 
- Accept software license terms and agree to Windows 10 setup
- Creat installation media and choose the ISO file option

![win10 download](https://github.com/resii-tech/NessusEssentialsLab/assets/129999089/b6fd88b5-ff83-425e-89bb-db1c4fa818f6)

## Part 1: Download and Install [Nessus Essentials](https://www.tenable.com/products/nessus/nessus-essentials)
- Register for an account
- You will receive an activation code in your email
- Copy your activation code in your email and click the Download Nessus as well
- Choose Nessus > View Downloads 
- Under version chose Nessus - 8.15.6
- Under platform choose Windows - x86_64 **make sure it includes Server 2012 R2, 7,8,10**
- Open Tenable Nessus and click next and install
- After finishing installation and window should pop up with a local host URL 
- Copy/paste the URL for future use 

## Part 2: Initialize Nessus Installation 
- Click Connect via SSL on web page 
- Click **Advance** on warning page > **Continue to localhost** > Nessus Essentials 
- Click skip on Get an activation code (we already have ours)
- Paste activation code > continue 
- Create a username and password **remember for future use**
- Click submit > and wait for download

![image](https://github.com/resii-tech/NessusEssentialsLab/assets/129999089/5e5a4544-9b9b-4ee5-b89f-f6e50d2d8934)

> NOTE: Copy and save the local URL for Nessus so it is easy to access in the future.
    * **Nessus URL:**  https://localhost:8834/# 

### Part 3: Find the IP Address of the Domain Controller

1. Go to the **DC** virtual machine, and log in.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/d22af83c-6d69-4bfd-b3f4-0e3fffecbb47" height="80%" width="80%"/>
</br>

2. Click **Start**, and type cmd.

3. Hit **Enter** to open **Command Prompt**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/bbb2a3c4-83c1-4d00-8ae6-6c497f3ed07e" height="80%" width="80%"/>
</br>
</br>

4. Type **ipconfig** into Command Prompt, and hit **Enter**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/d8d69dcb-ac33-4705-949d-cd13e30d20ad" height="80%" width="80%"/>
</br>
</br>

5. Find the address next to **IPv4 Address**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/be8e4267-95e5-4385-b0c0-ad8fc45042b1" height="80%" width="80%"/>
</br>
</br>

### Part 4: Create a New Scan in Nessus

1. Go back to the **PC 1** virtual machine.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/baa96c4c-0bd1-473c-af1b-58576a131584" height="80%" width="80%"/>
</br>
</br>

2. Click **Create a new scan** in Nessus.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/8208c32f-d8e2-44a5-9aeb-ad7b0198ce2e" height="80%" width="80%"/>
</br>
</br>

3. Click **Basic Network Scan**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/202622bc-2fb4-43f8-9134-01bb3083c153" height="80%" width="80%"/>
</br>

4. In the box next to **Name**, type in a name for your scan.

5. In the box next to **Targets**, type in the IP address of your domain controller.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/22deeb57-a232-4a89-aef0-9df44cf58450" height="80%" width="80%"/>
</br>
</br>

6. Click **Save** to save your scan.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/f4f61d26-f7fa-436e-aeb4-6ff59e55c976" height="80%" width="80%"/>
</br>
</br>

### Part 5: Run Scan 1 (Non-credentialed) and View the Results

1. From the **My Scans** page, click the Launch icon to begin running your scan.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/efaa5130-8ccf-4135-b27d-3caed00e4491" height="80%" width="80%"/>
</br>
</br>

2. Wait for the scan to finish. You will see a check mark next to the scan once it is complete.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/ae7a5f53-7cb9-4f6e-8c5f-06950c05027f" height="80%" width="80%"/>
</br>
</br>

3. Double click the scan to view the results.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/b7c54af8-20d6-4c42-9cbe-12126cb7579f" height="80%" width="80%"/>
</br>
</br>

4. In the **Host** tab you will see a bar that displays the vulnerability count and graphical depiction of the vulnerabilities that were found based on percentage.
   * Vulnerabilities are categorized based on their risk level (Critical, High, Medium, Low, Info).

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/5add47d0-3a51-4ac2-8575-a52204700cb0" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/8a198ff1-37f7-469e-84fc-0b8f66959091" height="80%" width="80%"/>
</br>
</br>

5. Click the bar that displays the vulnerability count to open up a detailed list of specific vulnerabilities that were found.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/08819803-e45d-42df-86b9-b798f549405f" height="80%" width="80%"/>
</br>
</br>

6. Click each vulnerability to see more details, including a description and proposed solution for the vulnerability.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/4c2ff102-109c-44f3-af85-f4a961789290" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/e0f0ff21-6a6e-4f50-9859-02536596d384" height="80%" width="80%"/>
</br>
</br>

### Part 6: Run Scan 2 (Credentialed) and View the Results

1. Go back to the **My Scans** page.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/aed078c9-af41-49b6-9c04-f2bffb1a5fe3" height="80%" width="80%"/>
</br>
</br>

2. Click the check box next to the scan.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/eaa126ea-a1b0-46f8-9a67-68313f572018" height="80%" width="80%"/>
</br>
</br>

3. Click the dropdown arrow next to **More**, and click **Configure**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/a3b34a4d-e657-4d9d-9d50-38b53e71e95e" height="80%" width="80%"/>
</br>
</br>

4. Select the **Credentials** tab, and click **Windows**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/8d467d39-332a-456e-8b97-e788ef34271f" height="80%" width="80%"/>
</br>
</br>

5. Type in the username and password for the admin account you created in Active Directory, and click **Save**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/cbf02f94-1770-4b6f-bbf6-0f6fc011077e" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/d424db6a-26dc-44ea-b7ee-5ad87a8fcaf1" height="80%" width="80%"/>
</br>
</br>

6. Go back to the **My Scans** page, and click the **Launch** icon next to the scan to run it.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/f61c610e-dad6-415b-b55b-51fd21332a2c" height="80%" width="80%"/>
</br>
</br>

7. Double click the scan to view the results.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/a991a81a-39bc-42e2-8b99-95179ed04868" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/9f12d9c3-2c55-45ec-8cfd-5504a6b926f2" height="80%" width="80%"/>
</br>
</br>

8. Select the **History** tab to view a list of scans you have completed.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/4b1fb242-d4de-4d9d-aac8-c40afe44f1b1" height="80%" width="80%"/>
</br>

9. Click each scan to review the results.

10. Compare the results of the non-credentialed scan to the results of the credentialed scan you just ran.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/ee3e0be2-10a8-43f8-bdd2-7de23cc223c7" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/7bdea49d-d7a8-4901-a607-76523ffa9bdb" height="80%" width="80%"/>
</br>
</br>

11. Select the **Vulnerabilities** tab to see a detailed list of vulnerabilities that were found after the credentialed scan.
    * The credentialed scan should find a lot more vulnerabilities than the non-credentialed scan.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/4eba4023-6d47-4a49-8e70-1b91b3775ebb" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/64b1119c-3bf0-4a14-b96c-960302cabe6c" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/7ef5a035-6efa-4c87-b082-e90a8dab419d" height="80%" width="80%"/>
</br>
</br>

### Part 7: Update Windows Server 2022 on the Domain Controller

1. Go back to the **DC** virtual machine, and log in.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/c9c75a24-5c98-48c3-bdb6-6f413f80954c" height="80%" width="80%"/>
</br>
</br>

2. Click **Start**, and select **Settings**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/997bf378-da9d-47fe-bf14-68364dc89c2c" height="80%" width="80%"/>
</br>
</br>

3. Click **Updates and Security**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/4202ccb9-2336-4992-b6f4-f23804ef32f5" height="80%" width="80%"/>
</br>
</br>

4. Click **Windows Update**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/08b186a9-6e0d-461e-88e3-a1d06d8be9c0" height="80%" width="80%"/>
</br>
</br>

5. Click **Install now** to install the updates.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/9c8c1c7d-03e3-47df-9a71-3aa1c02419db" height="80%" width="80%"/>
</br>
</br>

6. Keep installing updates until your system is completely up to date.
   * You may be required to restart the machine several times to apply the updates.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/c98f04b5-4c45-41fa-94ed-e9aae9305ca2" height="80%" width="80%"/>
</br>
</br>

### Part 8: Run Scan 3 (Credentialed) and View the Results

1. Go back to the **PC 1** virtual machine and log in.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/290cd7bc-572f-478f-bd4f-41e38b72ab75" height="80%" width="80%"/>
</br>
</br>

2. From the **My Scans** page inside Nessus, click the **Launch** icon to begin running your second credentialed scan.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/a07b3551-2218-41ac-a807-2f9b9eff69b9" height="80%" width="80%"/>
</br>
</br>

3. Once the scan is complete, double click it to review the results.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/b89a5511-4b7b-4a1f-966b-370a9bfda8e4" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/98d88351-8b3c-4283-9887-8bb51fd6b2f5" height="80%" width="80%"/>
</br>
</br>

4. Select the **History** tab to compare the results of the three scans.
   * Most of the vulnerabilities found after the first credentialed scan should now be fixed.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/b4463f02-4dee-486d-a936-670156fbc168" height="80%" width="80%"/>
</br>
</br>

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
