<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create two virtual machines (Client-1 and DC-1)
- Install Active Directory on the DC-1 VM
- Create a domain admin user within the domain
- Join the Client-1 VM to the domain of the DC-1 VM
- Setup remote desktop for non-administrative users on the Client-1 VM
- Create many additional users & log into the Client-1 VM with one of the new users

<h2>Deployment and Configuration Steps</h2>


![image](https://github.com/user-attachments/assets/236c5e9c-584b-42fa-82eb-15edee8b4f02)
To begin, we create two virtual machines (VMs): one named DC-1, which runs Windows Server 2022, and another named Client-1, running Windows 10. Both VMs are connected to the same virtual network (VNet) to enable communication. After deployment, we configure a static IP address on DC-1, replacing the default dynamic IP. This change allows Client-1 to use DC-1 as its DNS server, enabling it to locate and join the domain managed by the domain controller.


![image](https://github.com/user-attachments/assets/af0bca5f-146e-4621-998d-01a34bb50628)
Next, we access the domain controller (DC-1) using Remote Desktop Protocol (RDP). Once connected, we proceed to disable the Windows Defender Firewall for all network profiles—Domain, Private, and Public. To do this, we right-click the Start button, select Run, type wf.msc, and press Enter. In the Windows Defender Firewall window, we verify that the firewall is turned off for each profile.



![image](https://github.com/user-attachments/assets/784ec5c6-3679-4019-bf5a-9d10d0c40553)
We then update the DNS server setting on Client-1, assigning it the static private IP address of DC-1 via the network configuration in the Azure portal. To ensure the new DNS settings take effect, we proceed to restart both virtual machines.



![image](https://github.com/user-attachments/assets/8707331c-e4ac-4b4c-93f8-d21f9ef20eb3)




![image](https://github.com/user-attachments/assets/a259be4e-d527-4c38-bb01-4c9141c80897)
After both virtual machines have restarted, we connect to Client-1 via RDP. From within PowerShell, we ping the IP address of DC-1 to verify network connectivity—this should succeed because the firewall on DC-1 has been disabled. Next, we run the ipconfig /all command on Client-1 to confirm that DC-1's IP address is correctly set as the DNS server for the VM.


![image](https://github.com/user-attachments/assets/b002fb54-faf9-4378-bc53-aff513a240c1)
Proceed by installing Active Directory Domain Services on the domain controller (DC-1) using Server Manager."


![image](https://github.com/user-attachments/assets/f6f51703-a67e-4ad9-b08f-4677e6713cc7)
Next, we promote DC-1 to a Domain Controller by creating a new forest with the domain name 'mydomain.com'."



![image](https://github.com/user-attachments/assets/de2804ed-b9de-4119-8410-fce1af6d1254)
"
Next, we launch Active Directory Users and Computers and create two organizational units named _EMPLOYEES and _ADMINS by right-clicking 'mydomain.com', selecting 'New', and then choosing 'Organizational Unit'."


![image](https://github.com/user-attachments/assets/d3514ca2-54b5-4c89-8898-22b29a7c20e8)

Within the _ADMINS organizational unit (OU), we create a new user account for Jane Doe with the username 'jane_admin'."


![image](https://github.com/user-attachments/assets/4d567567-fcad-4146-bd60-bcdbb998f026)

![image](https://github.com/user-attachments/assets/cb729ef7-58ec-4f8b-a4e8-996ff30aa16c)

Within the _ADMINS organizational unit (OU), we create a new user account for Jane Doe with the username 'jane_admin'."


![image](https://github.com/user-attachments/assets/b8378745-7dbd-4a27-85d2-1a4139ee4569)

Next, from the Client-1 VM, we initiate the domain join process. After connecting via RDP, we right-click the Windows logo, select 'System', and click 'Rename this PC (Advanced)'. We then choose 'Change', select 'Domain', enter 'mydomain.com', and restart the VM to apply the changes."


![image](https://github.com/user-attachments/assets/86a2080a-be1d-4480-a99c-7f40075b943a)

"We then return to DC-1, open Active Directory Users and Computers, and create a new organizational unit named _CLIENTS under the mydomain.com domain.

![image](https://github.com/user-attachments/assets/3587f68f-b39e-408f-9103-b44d02727063)

"Next, we log into the Client-1 VM using the jane_admin account. We right-click the Windows logo, go to 'System', and select 'Remote Desktop'. Then, we click 'Select users that can remotely access this PC' and add domain users to grant them RDP access."

![image](https://github.com/user-attachments/assets/50befb99-271f-43b2-802d-13fc4f33c772)

We access PowerShell ISE on DC-1 with administrative rights under the jane_admin account, paste in the script from Josh Madakor's GitHub, and execute it to generate thousands of user accounts automatically."


![image](https://github.com/user-attachments/assets/1a084fe3-62e5-4628-be55-e69733dbebcd)

After executing the script, we open Active Directory Users and Computers to confirm that the accounts have been successfully created in the _EMPLOYEES organizational unit."

![image](https://github.com/user-attachments/assets/43afadfc-6c20-4e80-829c-3b0d2a544475)

"Finally, we log into the Client-1 VM using one of the accounts created by the script, using the username and default password 'Password1'. Once logged in, we open PowerShell to confirm that we are logged in as one of the script-generated users."



