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
In this step shown, I have created a Directory Services Restore Mode (DSRM) password. Once those steps have been completed, I continued through the remainder of the steps and ensured that that domain name stayed as "thebestdomain". 
</p>
<br />

<p>
<img src="https://i.imgur.com/xizGuL2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once completed, the virtual machine needed to restart to complete the installation of the system. As such I relaunched the virtual machine from Microsoft Azure. I tested the set up of the domain by loging in with "thebestdomain\ws-domaincontroller" as the username through the remote desktop. When logged in, I opened command line in the virtual machine and used the command "whoami" to again confirm that the output is "thebestdomain\ws-domaincontroller". This is shown in the screenshot. 
</p>
<br />


<p>
<img src="https://i.imgur.com/lJJtyG5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
For this next step I remote in to the Domain Controller of the Virtual Machine, and select the start menu, then select and open "Active Directory Users and Computer". 
</p>
<br />


<p>
<img src="https://i.imgur.com/zUVamJO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
For this next step I create an organization unit. First I right-click on the domain name, then select "new", then "organizational unit". 
</p>
<br />


<p>
<img src="https://i.imgur.com/ObKAWaB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
The organizational unit is essentially just a folder that will be holding all of the user account. The organizational unit that I created is named "_Employees". Next, I created another organizational unit and named it "_Admins". In a real-world scenario, the admin role would be assigned to an employee but this is just for illustrative purposes. 
</p>
<br />

<p>
<img src="https://i.imgur.com/REzJ3wv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this next section, I created a user in the "_Admins" folder that I created. The way to do this is to click on the "_Admins" folder on the left-side of the screen, and then when the screen pops up fill in the requested information and then hit "Next>".
</p>
<br />

<p>
<img src="https://i.imgur.com/2eEAeU9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this next screen, I filled in the default password information. In the real world, this would normally be preset by the main administrator. It's important to note that in this example the password will never expire but in a real-word scenario, an admin would want to select "User must change password at next logon", to ensure that a secure password is established by the user.
</p>
<br />

<p>
<img src="https://i.imgur.com/iCoBen7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this next step, I verified that the User "John Doe" reflected under admins, then I proceed to right-clicking on the name and selected "Properties". Under properties, I navigated to "Member of", and then selected "Add", this step enables you to add the user to a group list. I looked up and added, "domain admins", this role allows any member to make changes to the domain controller. Once I verified that Domain Admins appeared, I clicked on "Apply" to apply the change and then "OK". 
</p>
<br />

<p>
<img src="https://i.imgur.com/Dcwe6Um.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this next step, I tested to ensure that I was able to log in with the newly created account. I used the previously created "john_doe" as the username and the password that I created in an earlier step. 
</p>
<br />

<p>
<img src="https://i.imgur.com/HNBz67p.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
For further verification that I was successfully logged in to the correct account. I opened command prompt, then typed in "hostname" to verify that this was the Domain Controller Virtual Machine, and then "whoami" to verify that the output would be "thebestdomain\john_admin.
</p>
<br />

<p>
<img src="https://i.imgur.com/oKqyc4G.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
The next step is to connect the Domain Controller and the Windows 10 Client Virtual machines. This would make the users able to access the domain controller from the client virtual machines. The way to do this would be to update the DNS settings in the Client VM to point to the private static IP from the Domain Controller. To find that you need to go to your Domain Controller Network Settings on Microsoft Azure. For this example, the Private IP Address of the Domain Controller is 10.0.0.5
</p>
<br />


<p>
<img src="https://i.imgur.com/6piKgfb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, now that I have verified the private IP address of the Domain Controller VM, I opened the Client VM in Microsoft Azure and went to "Settings", "DNS Servers", and then changed the DNS servers to "Custom", added 10.0.0.5, and then saved. This will make your client VM restart. 
</p>
<br />


<p>
<img src="https://i.imgur.com/GUgwHTv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next I verified that the DNS server settings were updated successfully in Microsoft Azure. To do that, I remoted in to the Client VM and opened the command line. In the command line I typed in "ipconfig /all" and then scrolled down to look for DNS Servers. If correct, this should display the private IP from your domain controller. If yours does not reflect the IP address that you were expecting to see, you would need to close the RDP session, and restart the VM in Microsoft Azure and try again. 
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
