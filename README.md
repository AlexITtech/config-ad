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
