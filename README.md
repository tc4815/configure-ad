<p align="center">
<img src="https://i.imgur.com/7xLtdix.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed utilizing Microsoft Azure </h1>

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
When setting up resources in Azure, create the Domain Controller Virtual Machine (Windows Server 2022) named “DC-1”. Take note of the Resource Group and Virtual Network (Vnet) that get created at this time. Go to Networking -> Network Interface and click on the NIC number (see above).

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
Now you can right-click on the Start button and select "System". Click "Rename this PC" then click the "Change" button. Rename it under Domain to "mydomain.com" as seen in the steps numbered above.
</p>
<br />

<p>
<img src="https://imgur.com/kCe87OE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
When prompted, enter the domain controller credentials you set in the previous steps. Client-1 will then need to restart itself.
</p>
<br />

<p>
<img src="https://imgur.com/tGR75X0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log back into Client-1 using "mydomain.com\jane_admin" and the associated admin password this time. Right click the Start button again and select "System" -> "Remote Desktop" -> "Select users that can remotely access this PC". Click "Add" and type "Domain" to seach and add the group named "Domain Users". This allows you to add a group of users instead of adding each user one at a time. Click "OK" to apply the new persmission. 
</p>
<br />
 
<p>
<img src="https://imgur.com/1U8gK74.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Back in DC-1 using Active Directory Users and Computers, you can now see that users have been added by clicking on "Users" -> "Domain Users" -> "Members" tab as seen above.
</p>
<br />
 
<p>
<img src="https://imgur.com/TC6CwSZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Still in DC-1, launch PowerShell_ise as an admin, create a new file and paste the contents of the following script into it. The script used can be found here: https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1 - this script creates 1000 accounts with the same password ("Password1") within the "_EMPLOYEES" organizational unit. Run the script and observe the accounts being created as can be seen above. 
</p>
<br />
 
<p>
<img src="https://imgur.com/fSK6Uhi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Back in Active Directory Users and Computers you can now observe all of the users created in the previous step within the "_EMPLOYEES" organizational unit.
</p>
<br />
 
<p>
<img src="https://imgur.com/K87BgeP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now you are able to choose any of the users created in the previous steps and login to Client-1 using their new credentials. The example above and below shows the user "bat.hupapa" with username: "mydomain.com\bat.hupapa" and password: "Password1".
</p>
 
<p>
<img src="https://imgur.com/IctlhRx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
The above example shows proof of login and hostname of the user "bat.hupapa" once logged into the Virtual Machine "Client-1". This concludes the tutorial of deploying on-premises Active Directory utilizing Virtual Machines - a domain controller and client(s) - within Microsoft Azure. 
</p>
<br />
 
