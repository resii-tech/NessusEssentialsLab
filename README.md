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
   * [Continue Set Up: Download Windows 10 ISO file](#Part-1-Download-Windows-10-ISO-file)
   * [Part 2: Install Nessus on the PC 1 VM](#Part-2-Install-Nessus-on-the-PC-1-VM)
   * [Part 3: Find the IP Address of the Domain Controller](#Part-3-Find-the-IP-Address-of-the-Domain-Controller)
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

### Part 1: Download Nessus on the PC 1 VM

1. Start up the **DC** virtual machine and the PC 1 virtual machine.
2. Log in to the **PC 1** virtual machine using the admin account you created in Active Directory.
3. Open **Microsoft Edge**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/71f1b919-4fa3-460b-a507-66b049a32143" height="80%" width="80%"/>
</br>
</br>

4. Go to the following link: https://www.tenable.com/downloads/nessus 

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/b009fcfd-871c-4659-adc0-cede46fe6750" height="80%" width="80%"/>
</br>
</br>

5. Under **Version**, make sure the latest version of Nessus is selected.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/e16af465-0a76-43f6-97e7-ed5767cad7df" height="80%" width="80%"/>
</br>
</br>

6. Under **Platform**, make sure **Windows - x86_64** is selected.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/c589b325-bd71-46b0-8d94-fce50e2912dc" height="80%" width="80%"/>
</br>
</br>

7. Click **Download**, and click **I Agree on the License Agreement**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/3f0a9c49-0365-4416-b9c1-f2257a8090e3" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/3d599c82-998a-45b5-92e4-bbaac74f9f9d" height="80%" width="80%"/>
</br>
</br>

### Part 2: Install Nessus on the PC 1 VM

1. Once Nessus has finished downloading, double click the file to start the installation process

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/2174705c-2ce3-4ad3-b1b8-613fe4824fd0" height="80%" width="80%"/>
</br>
</br>

2. Once the installation wizard pops up, click **Next**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/5af3e61a-d40d-4e13-af3b-f16dc4a36d6c" height="80%" width="80%"/>
</br>
</br>

3. Select **I accept the terms in the license agreement**, and click **Next**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/a21c40e6-f568-4181-85f4-2d81153b7c3a" height="80%" width="80%"/>
</br>
</br>

4. Click **Next** again, and click **Install** on the last page.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/0c952c1a-392a-427e-8827-1563d10a9322" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/8bc50846-763a-4456-bf58-75cd94703d64" height="80%" width="80%"/>
</br>
</br>

5. Click **Yes** when asked “Do you want to allow this app to make changes to your device?”.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/c5136c55-3be1-4353-8f8b-a3af4da246e0" height="80%" width="80%"/>
</br>
</br>

6. Once Nessus has finished installing, click **Finish**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/4d5154a3-b6ec-4173-90c9-77d6bf57cb7d" height="80%" width="80%"/>
</br>
</br>

7. A new page will open up in **Microsoft Edge**. Click **Connect via SSL**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/d13772ba-c239-4966-bb3f-949c0cb1484b" height="80%" width="80%"/>
</br>
</br>

8. Click **Advanced**, and click **Continue to local host (unsafe)**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/03988b42-2090-4de6-9b65-50afd5b113b5" height="80%" width="80%"/>
</br>
</br>

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/308402cf-afe1-48e2-ad9a-1a65bc2d7aca" height="80%" width="80%"/>
</br>
</br>

9. Once Nessus has finished initializing, click **Continue**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/50fd220a-6af1-4574-9480-5492ed31f279" height="80%" width="80%"/>
</br>
</br>

10. Select **Register for Nessus Essentials**, and click **Continue**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/3707d0d8-bdc0-4e7d-8296-6c6e0b3633fb" height="80%" width="80%"/>
</br>
</br>

11. Under **Get an activation code**, type in your first name, last name, and email.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/94d7f073-49bf-4ba5-a5ea-6a15a11e748f" height="80%" width="80%"/>
</br>

12. Click **Register**, and an activation code will be sent to your email.

13. Find the activation code that was sent to your email.

14. Enter the code in the box under **Activation Code**, and click **Continue**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/0d0fbfba-6e2e-4ce6-9e76-f2f3e9c585ba" height="80%" width="80%"/>
</br>
</br>

15. On the **License Information** page, click **Continue** again

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/6de76f5a-3823-4da2-bc10-5a6e0d393536" height="80%" width="80%"/>
</br>
</br>

16. Create a username and password for Nessus. Then click **Submit**.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/2d1518b2-5189-471e-b9d7-3fdd025227e6" height="80%" width="80%"/>
</br>
</br>

17. Wait for the plugins to finish downloading then you will be taken to the **My Scans** page inside Nessus.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/3468e112-c58c-4313-84e7-cf0f3c8c096f" height="80%" width="80%"/>
</br>
</br>

18. Before you can create your first scan, you will have to wait for the plugins to finish downloading.
    * You can hover your mouse over the spinning circle icon in the top menu bar to track the progress.

<img src="https://github.com/emann615/MicrosoftSentinelLab/assets/117882385/6ca75103-8447-4a28-9925-087b79b9df0f" height="80%" width="80%"/>
</br>
</br>

19. Copy and save the local URL for Nessus so it is easy to access in the future.
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
