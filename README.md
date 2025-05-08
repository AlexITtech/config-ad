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
