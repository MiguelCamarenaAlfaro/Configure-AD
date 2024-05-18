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
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- I will create and configure the resources necessary to implement active directory. Primarily, I will be creating a client virtual machine running Windows 10 OS, and a domain controller VM running Windows Server 2022. 
- I will ensure that there is a connectivity between the client and domain controller vms through the ping command line request.
- I will install and configure active directory on the domain controller virtual machine. 
- I will create an administrator account and an end user account in active directory and test the accounts. 
- I will joing the client virtual machine to the domain that I created in Active directory.
- I will set up remote desktop for non-administrative users on the client virtual machine.
- I will deploy additional users in active directory, paying special attention to configuration and test implementation by attempting to log into the client virtual maching as one of those users. 

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/orV8SlV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this initial step, I created a resource group in Microsoft Azure, I will be subsequently adding the client and the domain controller virtual machines to this resource group to maintain a high-level of organization.
</p>
<br />

<p>
<img src="https://i.imgur.com/bwv2kjE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Here, I created and configured the client virtual machine. The VM is running windows 10 22H2. It's important to ensure that in the "network tab" of the configuration process the RDP port 3389 is enabled. If disabled, using remote desktop to this virtual machine is impossible. Additionally, we ensure that the virtual machine is configured inside the resource group that we created previously.
</p>
<br />

<p>
<img src="https://i.imgur.com/9VIDapn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
What is shown here is that we have validated that the RDP over port 3389 is enabled, this will allow us to remote into the virtual machine. Next, we need to take special note of the Vnet, as we will need to ensure that the domain controller is set up on the same Vnet. 
</p>
<br />


<p>
<img src="https://i.imgur.com/eQqAGEm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
The next step is to create the virtual machine for the domain controller. In this step I configured the virtual machine through the Microsoft Azure portal to run Windows Server 2022. Special attention was given to the resource group and the region to ensure that they matched with the resource group and the region that were configured on the client virtual machine running Windows 10. 
</p>
<br />


<p>
<img src="https://i.imgur.com/ur5V0Lv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Here, I ensured that RDP over port 3389 was enabled for the domain controller and that the Vnet configured on this machine is the same Vnet that was configured on the client virtual machine running windows 10. 
</p>
<br />


<p>
<img src="https://i.imgur.com/lhOfzPq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
When working with a domain controller we need to validate that the IP is set to static. Using Microsoft Azure the IP is typically set to Dynamic be default unless there is a request for Static. To update the IP protocol in the domain controller, first we would need to review the domain controller VM in Microsoft Azure, then navigate to "Network Settings", then "IP Configurations", then click on "ipconfig", then in the "edit IP Configuration" menu we would need to select "static" under "Private IP Address" setting. Before clicking on "save" we will need to take note of the private IP address and the Public IP address shown as we will need them to continue in the future steps. 
</p>
<br />


<p>
<img src="https://i.imgur.com/slfMOOJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To test the connectivity between the client virtual machine and the domain controller virtual machine I used remote desktop to access the client virtual machine, then I opened command line and tested by pinging the static IP for the domain controller. Originally this failed, reason being that ICMPv4 was disabled in the domain controller. To resolve this, I used remote desktop to access the domain controller and I navigated to the inbound firewall rules to enable ICMPv4 on the domain controller. After completing this firewall configuration change, I retested the ping command on the client virtual machine and received a successfull ping response. 
</p>
<br />


<p>
<img src="https://i.imgur.com/dIcxf7I.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the Active directory set up, I selected "Add roles & Features", then "Role-based or feature-based installation, then in "server selection", I select "select a server from the server pool", ensuring that my domain controller and private IP address are highlighted. 
</p>
<br />


<p>
<img src="https://i.imgur.com/xQ5EPdo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After going to through the menus, I selected and checked "Active Directory domain services" and proceed through the remaining steps. This prompts the windows server virtual machine to install Microsoft Active Directory Domain Services (AD-DS)
</p>
<br />


<p>
<img src="https://i.imgur.com/2iNnk1p.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After confirming that the installation of Microsoft Active Directory Domain services was completed, the next thing I completed was clicking on the alert in the flag icon, and clicking on the "Promote this server to a domain controller".
</p>
<br />


<p>
<img src="https://i.imgur.com/1eREoRk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this next step, I selected "create a new forest" and then created a new domain name. For this example, the domain that I created is "thebestdomain.com"
</p>
<br />


<p>
<img src="https://i.imgur.com/t8FMEL2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this step shown, I have created a Directory Services Restore Mode (DSRM) password. Once those steps have been completed, I continued through the remainder of the steps and ensured that that domain name stayed as "thebestdomain". Once completed, the virtual machine needed to restart to complete the installation of the system. As such I relaunched the virtual machine from Microsoft Azure.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

