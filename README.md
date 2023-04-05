<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Microsoft Azure) - </h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>:construction:   UNDER CONSTRUCTION  :construction:<h2>
<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Set up resources in Azure
- Ensure connectivity between the Client-1 and Domain Controller (DC-1)
- Install Active Directory
- Create and Admin and Normal User account in Active Directory
- Join Client-1 to Domain
- Set up Remote Desktop for Non-Administrative users on Client-1
- Set up additional users and log into Client-1 using a created user
- Finish.

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://imgur.com/3BdIqPg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
When setting up resources in Azure, create the Domain Controller VM (Windows Server 2022) named “DC-1”. Take note of the Resource Group and Virtual Network (Vnet) that get created at this time. Go to Networking -> Network Interface and click on the NIC number (see above).

</p>
<br />

<p>
<img src="https://imgur.com/QQlwzzy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Set Domain Controller’s NIC Private IP address to be static (see above). Then create a VM ("Client-1") within the same Resource Group and network as the Domain Controller.
</p>
<br />

<p>
<img src="https://imgur.com/u5O6xfh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Using Microsoft Remote Desktop (for MacOS as seen above), login to Client-1 VM using it's public IP address. Use the username ("labuser" in this case) and password you created when set-up the VM "Client-1".
</p>
<br />

<p>
<img src="https://imgur.com/tepWnu1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Using Command Prompt, ping DC-1’s private IP address (found via Azure) utilizing the command ping -t <ip address> (perpetual ping). You should see "Request Timed Out" due to DC-1's firewall blocking ICMP-v4 traffic (as seen above).

</p>
<br />

<p>
<img src="https://imgur.com/oJ9iBnB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to DC-1 using the same Microsoft Remote Desktop (if on MacOS) and open Windows Defender Firewall (search in windows once logged in to DC-1). Locate and enable ICMPv4 in on the local windows Firewall (see above).
</p>
<br />

<p>
<img src="https://imgur.com/JUjGJN7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Switch back to Client-1 and observe traffic in Command Prompt is now flowing between Client-1 and the Domain Controller (DC-1). The ping has thus suceeded (as shown above). 
</p>
<br />

<p>
<img src="https://imgur.com/gzx1Lbi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log back in to DC-1 and launch the Server Manager dashboard if not already open. Install Active Directory Domain Services. Make sure "Active Directory Domain Services" is selected. Click through prompts to install Active Directory onto Domain Controller (DC-1).
</p>
<br />

<p>
<img src="https://imgur.com/DrUiM0X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After you install Active Directory Domain Services, promote it as a DC by clicking on the yellow flag (top right). Make sure you "add a new forest" and name it anything (in this case I selected "mydomain.com" as seen above). The VM will then need to reboot / re-establish connection with the DNS. 
</p>
<br />

<p>
<img src="https://imgur.com/g0Uqqgc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log back into the DC-1 VM only this time using the credentials created when promoting it to a Domain Controller. Use "mydomain.com\labuser" as the username instead of "labuser". Use your original password when you first created DC-1.
</p>
<br />

<p>
<img src="https://imgur.com/vu2pzLG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 Once back in DC-1, open "Active Directory Users and Computers" from the Search/Start menu and create a new Organizational Unit (OU) named "_EMPLOYEES" and another one named "_ADMINS".
</p>
<br />

<p>
<img src="https://imgur.com/EiHGGJJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Also using "Active Directory Users and Computers", create a new employee named Jane Doe with username of "Jane_Admin". 
</p>
<br />

<p>
<img src="https://imgur.com/9PRATnI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once created, right click "Properties" on Jane Doe and add the new employee to the "Domain Admins" security group as seen above. Log out of DC-1's remote desktop connection and log back in as "mydomain.com\jane_admin". This will be your admin account from now on.
</p>
<br />

<p>
<img src="https://imgur.com/lFZYPBb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we need to join Client-1 to our domain (DC-1 or "mydomain.com"). In the Azure portal, set Client-1's DNS settings to the DC-1's Private IP address as seen above. Be sure to click "Save" button at the top. Restart Client-1 from it's main page in the Azure portal. Log back into Client-1 as the original local admin ("labuser") and join it to the domain (computer will restart).
</p>
<br />

<p>
<img src="https://imgur.com/2dwTNmQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once logged back into Client-1, launch Command Prompt and type "whoami" and you should now be able to see DC-1's Private IP address now as shown above.
</p>
<br />

<p>
<img src="https://imgur.com/x9SlLDN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now you can right-click on the Start button and select "System". Click "Rename this PC" then click the "Change" button. Rename it under Domain to "mydomain.com". Client-1 will then need to restart again.
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
 
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
