<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Windows Server 2022 on Azure platform
- Setup Windows 10 client on Azure platform
- Ensure Connectivity
- Install Active Directory
- Configure Active Directory

<h2>Deployment and Configuration Steps</h2>

![Screen Shot 2023-09-19 at 9 23 37 PM](https://github.com/NickAumer/Azure-Active-Directory/assets/145170622/11e931c5-fd7d-4793-9799-8943ce5f9b62)

<p>
In azure create a new virtual machine running windows server 2022. This is our domain controller, take note the resource group and virtual network created at this time. Set the NIC of the domain controller to static. Ensure the windows 10 VM you make in azure is on the same resource group and network as the domain controller. 
- do this by going to the network tab when creating the VM and selecting the network you created earlier.
Ensure they are on the same network by logging into the client VM via remote desktop and running a perpetual ping to the domain controllers private IP address. ping -t (private-ip).
Then login to the domain controller and enable ICMPv4 on the LOCAL firewall. Go back to the client afterwards to see the ping succeed. Once confirmed you can disable the policy and stop the ping
</p>
### Setting up Admin
<p>
Go back to the domain controller and install active directory domain services via the server manager. 
</p>

![Screen Shot 2023-09-19 at 9 29 24 PM](https://github.com/NickAumer/Azure-Active-Directory/assets/145170622/322e61c9-09b9-42ba-a228-04a48c023b4b)

<p>
Once installed promote to domain controller.
</p>

![Screen Shot 2023-09-19 at 9 32 46 PM](https://github.com/NickAumer/Azure-Active-Directory/assets/145170622/4099818e-b9ac-42f5-bd67-0e33054af830)
<p>
Create it as a new tree with the domain you want.
</p>

![Screen Shot 2023-09-19 at 9 33 38 PM](https://github.com/NickAumer/Azure-Active-Directory/assets/145170622/56fcdcfb-4ded-4a4a-a4c2-92cb43742fd5)
<p>
login to the Domain Controller with remote desktop as (thedomainyouset.com\YourRDPusername) and your RDP password. Once back on the server hit the windows key and type in active directory, we want Active Directory Users and Computers.
Once there right click on your domain and Create 2 new Organizational Units, 1 will be _EMPLOYEES and the other _ADMINS. Right click _ADMINS and create a new user name as needed and make sure to set the username and password to something you remember, this is our new admin account. Once created right click the user you just made, go to the member of tab and add them to the Domain Admins Security Group. Log out and RDP back in with the Admin you just created.
</p>

### Connecting the Client to the Domain Controller
<p> 
In the Azure portal go the to resource group containing the VM's and click on the clients VM, and from there go to its NIC page, once there click DNS settings and change to the private ip of the Domain Controller. Restart Client VM. Log back into client and go to advanced system properties, from there click change and change the DNS to the thedomainyoucreated.com. Restart client VM. Go back to Domain Controller and verify its connected under, Active Directory Users and Computers, Under the computer folder in the root of your domain. (you can make a new Organizational Unit called _CLIENTS and move the client there for organization purposes, not required). Now login to the Client with the admin user you created via mydomain.com\jane_admin . Open system properties, click remote desktop and allow DOMAIN users access to remote desktop. This step enables the client computer to be logged in to with a normal non-admin account. Usually done with a group policy but this is for example. and that is the basics of an active directory.
</p>
<br />

## CONGRATZ!
