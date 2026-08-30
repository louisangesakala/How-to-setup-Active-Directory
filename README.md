<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" height="40%" width="70%"alt="Microsoft Active Directory Logo"/>
</p>

<h1>Configuring Active Directory (On-Premises) Within Azure</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<!-- <h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com) -->

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 11

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Resources
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (myadproject.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>
<br />
<br />
<h3 align="center">Setup Resources in Azure</h3>
<br />
<h3>Step 1</h3>
<p>
  Create the Resource Group named “RG-AD-project”
</p>
<p>
  <img src="https://i.imgur.com/2kLXoHp.png" height="50%" width="50%" alt="resource group"/>
</p>
<h3>Step 2</h3>
<p>
  <b>Create a Virtual Network named "Active-Directory-VNet" for our resource group:</b>
</p>
<p>
  Type Virtual Network in the search bar and select Virtual Network to start the creation.
</p>
<p>
  <img src="https://i.imgur.com/PAE5XlO.png" height="50%" width="50%" alt="Virtual Network"/>
</p>
<p>
  <img src="https://i.imgur.com/AHSfUxS.png" height="50%" width="50%" alt="Virtual Network creation"/>
</p>
<p>
  <img src="https://i.imgur.com/SNDwPwD.png" height="50%" width="50%" alt="Virtual Network creation"/>
</p>
<h3>Step 3</h3>
<p>
  <b>Create the Domain Controller VM (Windows Server 2022) named “DC-1”:</b>
</p>
<p>
  Create the Virtual Machine named “DC-1”
</p>
  <img src="https://i.imgur.com/jurl3SL.png" height="50%" width="50%" alt="vm ms server"/>
</p>
<p>
  <img src="https://i.imgur.com/s3aYL3t.png" height="50%" width="50%" alt="vm ms server"/>
</p>
<p>
  <img src="https://i.imgur.com/d0vq28T.png" height="50%" width="50%" alt="vm ms server"/>
</p>
<p>
  In the Networking tab, use the same Resource Group and Vnet that was created in previous step (Active-Directory-VNet):
</p>
<p>
  <img src="https://i.imgur.com/tCpnRPs.png" height="75%" width="60%" alt="vm windows"/>
</p>
<p>
  <img src="https://i.imgur.com/ypVmh1f.png" height="75%" width="60%" alt="vm windows"/>
</p>
<h3>Step 4</h3>
<p>
  Create the Client VM (Windows 11) named “Client-1”.<br>
  Use the same Resource Group and Vnet that was created in previous step:<br>
</p>
<p>
  <img src="https://i.imgur.com/5HRJ4Sp.png" height="75%" width="50%" alt="vm windows"/>
</p>
<p>
  <img src="https://i.imgur.com/0QrbImg.png" height="75%" width="50%" alt="vm windows"/>
</p>
<p>
  <img src="https://i.imgur.com/1MdmCO1.png" height="75%" width="50%" alt="vm windows"/>
</p>
<h3>Step 5: Set Domain Controller’s NIC Private IP address to be static</h3>
<p>
  Navigate to the Virtual Machines page, and select the VM "D-1" we created in Step 3.
</p>
<p>
  On the left hand section, and click on the drop-down menu "Networking". Then click "Network Settings".
</p>
<p>
  <img src="https://i.imgur.com/IpbRBPn.png" height="75%" width="70%" alt="static ip"/>
</p>
<p>
  Next click on 'Network interface / Ip configuration", to start the configuration
</p>
<p>
  <img src="https://i.imgur.com/P4PAPHV.png" height="75%" width="70%" alt="static ip"/>
</p>
<p>
  Finally click on ipconfig1 -> in the next Edit IP Configuration page, change the Allocation from Dynamic to Static
</p>
<p>
  <img src="https://i.imgur.com/21rYrcI.png" height="75%" width="70%" alt="static ip"/>
</p>
<p>
  <img src="https://i.imgur.com/JJxFP1K.png" height="75%" width="70%" alt="static ip"/>
</p>
<p>
  Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher):
</p>
<p>
  <img src="https://i.imgur.com/ViRelV7.png" height="75%" width="80%" alt="topology"/>
</p>
<br />
<br />
<h3>Step 6</h3>
<h3 align="center">Ensure Connectivity between the client and Domain Controller</h3>
<br />
<p>
  Login to Client-1 with Remote Desktop -> open Powershell -> ping DC-1’s private IP address with ping
</p>
<p>
  <img src="https://i.imgur.com/7OuI7sa.png" height="75%" width="75%" alt="Client-1's public IP for Remmote Desktop"/>
</p>
<p>
  <img src="https://i.imgur.com/kT0Xkqv.png" height="40%" width="40%" alt="adding Client-1 to Remmote Desktop"/>
</p>
<p>
  <img src="https://i.imgur.com/LBmxGVT.png" height="75%" width="75%" alt="ping"/>
</p>
<p>
  Now, login to the Domain Controller with Remote Desktop.
</p>
<p>
  <img src="https://i.imgur.com/YYqwQPS.png" height="40%" width="40%" alt="ping"/>
</p>
<p>
  Navigate to the Windows Defender Firewall Advanced Security window
</p>
<p>
  On the left hand panel, click on Inbound Rules -> right-click Core Networking Diagnostics for ICMPv4 (Status: Private,Public), and enable it.
</p>
  
<p>
  <img src="https://i.imgur.com/jTz8Lmp.png" height="75%" width="75%" alt="Windows Defender Firewall"/>
</p>
<p>
  <img src="https://i.imgur.com/DaxtjOi.png" height="75%" width="75%" alt="inbound rules"/>
</p>
<p>
  <img src="https://i.imgur.com/yqDRICI.png" height="75%" width="75%" alt="enable ICMPv4"/>
</p>

<p>
  Check back at Client-1 to see the ping succeed:
</p>
<p>
  <img src="https://i.imgur.com/5X6Q0ok.png" height="75%" width="75%" alt="ping success"/>
</p>
<p>
  Success! the private DC-1's private IP address is now responding after we enabled ICMPv4 in the Windows Defender Firewall Advanced Security.<br>
  <b>Note: you can obtain the same results by simply deactivating Firewall in  Windows Defender Firewall Advanced Security.</b>
</p>
<p>
  Navigate to the Windows Defender Firewall Advanced Security.<br>
  -In the main window, click on Windows Defender Firewall Properties<br>
  -In the pop up window, navigate to each profile tab (Domain, Private, Public) and turn Firewall State from On(recommended) to Off.<br>
  -Finally click Apply and close the window,
</p>
<p>
  <img src="https://i.imgur.com/56MRhah.png" height="55%" width="55%" alt="Firewall Properties"/>
</p>
<p>
  <img src="https://i.imgur.com/E6UHnuE.png" height="55%" width="55%" alt="Firewall Properties"/>
</p>
<p>
  <img src="https://i.imgur.com/0YV9jlN.png" height="55%" width="55%" alt="Firewall Properties"/>
</p>
<p>
  Check back at Client-1 to see the ping succeed:
</p>
<p>
  <img src="https://i.imgur.com/ovCo1ok.png" height="55%" width="55%" alt="ping success"/>
</p>
<br />
<br />
<h3 align="center">Install Active Directory</h3>
<br />
<h3>Step 1: Login to DC-1 and install Active Directory Domain Services</h3>
<p>
  Login to DC-1, open Service Manager and click on Add Roles and Features
</p>
<p>
  <img src="https://i.imgur.com/tO0ddqX.png" height="55%" width="55%" alt="Add roles and features"/>
</p>
<p>
  In the Add Roles and Features pop-up window, click Next until you get to the Server Roles page, then select Active Directory Domain Server<br>
  A new pop up window should open -> click Add Features<br>
                                                                                                                                              
</p>
<p>
  <img src="https://i.imgur.com/FNvT1jr.png" height="55%" width="55%" alt="ping success"/>
</p>
<p>
  <img src="https://i.imgur.com/E7lzaWp.png" height="5%" width="55%" alt="active directory install"/>
</p>
<p>
  Continue to click next until you reachh the last window, then click install.
</p>
<p>
  <img src="https://i.imgur.com/ofCBYqj.png" height="55%" width="55%" alt="Add roles and features"/>
</p>
<br/>
<br/>
<h3>Step 2: Promote as a Domain Controller</h3>
<p>
  We're going to configure the Active Directory to becomme a Domain Controller, in what's called the new forest
</p>
<p>
 In the Server Manager, notice the yellow flag in the upper right corner of the windown.<br>
 Click on the yellow flag -> then click on "Promote this server as a domain controller"
                                                                                        
</p>
<p>
  <img src="https://i.imgur.com/4AY89R6.png" height="55%" width="55%" alt="domain controller promotion"/>
</p>
<p>
  <img src="https://i.imgur.com/wAZF493.png" height="55%" width="55%" alt="domain controller promotion"/>
</p>
<p>
  In the pop up window, Setup a new forest as mydomain.com (can be anything, just remember what it is)<br>
  Then click next until the last page, and finally click Install.<br>
  At the end of the installation, it should log you out your session and close the D1's remote desktop session.<br.
</p>
<p>
  <img src="https://i.imgur.com/1QjZqKk.png" height="55%" width="55%" alt="set new forest"/>
</p>
  <p>
  <img src="https://i.imgur.com/uQe4gxp.png" height="55%" width="55%" alt="set new forest"/>
</p>
    <p>
  <img src="https://i.imgur.com/U1ICAzk.png" height="55%" width="55%" alt="set new forest"/>
</p>
<p>
  Restart and then log back into DC-1 as user: mydomain.com\labuser (same password you use for user: labuser)
</p>
<p>
  <img src="https://i.imgur.com/k838Eea.png" height="55%" width="55%" alt="fqdn login"/>
</p>
<br />
<br />
<h3 align="center">Create an Admin and Normal User Account in AD</h3>
<br />
<p>
  In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” and another one called "_ADMINS":
</p>
<p>
  <img src="https://i.imgur.com/cYmv0r7.png" height="75%" width="100%" alt="organizational unit"/>
  <img src="https://i.imgur.com/v02CBPI.png" height="75%" width="100%" alt="organizational unit"/>
</p>
<p>
  Create a new employee named “Jane Doe” with the username of “jane_admin”:
</p>
<p>
  <img src="https://i.imgur.com/h546E6L.png" height="75%" width="100%" alt="admin creation"/>
</p>
<p>
  Add jane_admin to the “Domain Admins” Security Group:
</p>
<p>
  <img src="https://i.imgur.com/mnLwTgq.png" height="75%" width="100%" alt="security group"/>
</p>
<p>  
  Log out/close the Remote Desktop connection to DC-1 and log back in as “myadproject.com\jane_admin”. Use jane_admin as your admin account from now on:
</p>
<p>
  <img src="https://i.imgur.com/xWZ4Kol.png" height="75%" width="100%" alt="admin login"/>
</p>
<br />
<br />
<h3 align="center">Join Client-1 to your domain (myadproject.com)</h3>
<br />
<p>
  From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address:
</p>
<p>
  <img src="https://i.imgur.com/1KRsjI6.png" height="75%" width="100%" alt="client dns settings"/>
</p>
<p>
  From the Azure Portal, restart Client-1.
</p>
<p>
  Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart):
</p>
<p>
  <img src="https://i.imgur.com/50wszcP.png" height="75%" width="100%" alt="domain joining"/>
</p>
<p>
  Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.
</p>
<p>
  Create a new OU named “_CLIENTS” and drag Client-1 into there:
</p>
<p>
  <img src="https://i.imgur.com/vB1n9m0.png" height="75%" width="100%" alt="active directory client verification"/>
</p>
<br />
<br />
<h3 align="center">Setup Remote Desktop for non-administrative users on Client-1</h3>
<br />
<p>
  Log into Client-1 as mydomain.com\jane_admin and open system properties.
</p>
<p>
  Click “Remote Desktop”.
</p>
<p>
  Allow “domain users” access to remote desktop.
</p>
<p>
  You can now log into Client-1 as a normal, non-administrative user now.
</p>
<p>
  Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab):
</p>
<p>
  <img src="https://i.imgur.com/8BfpT3s.png" height="75%" width="100%" alt="remote desktop setup"/>
</p>
<br />
<br />
<h3 align="center">Create a bunch of additional users and attempt to log into client-1 with one of the users</h3>
<br />
<p>
  Login to DC-1 as jane_admin
</p>
<p>
  Open PowerShell_ise as an administrator.
</p> 
<p>  
  Create a new File and paste the contents of this script (https://github.com/Xinloiazn/configure-ad/blob/main/adscript.ps1) into it:
</p>
<p>
  <img src="https://i.imgur.com/0i8uApf.png" height="75%" width="100%" alt="create users script"/>
</p>
<p>
  Run the script and observe the accounts being created:
</p>
<p>
  <img src="https://i.imgur.com/6QOGzs6.png" height="75%" width="100%" alt="observe create users script"/>
</p>
<p>
  When finished, open ADUC and observe the accounts in the appropriate OU and attempt to log into Client-1 with one of the accounts (take note of the password in the script):
</p>
<p>
  <img src="https://i.imgur.com/ZZCfiCp.png" height="75%" width="100%" alt="employee user accounts"/>
  <img src="https://i.imgur.com/7gBpNzN.png" height="75%" width="100%" alt="employee user selection"/>
  <img src="https://i.imgur.com/cqsddjn.png" height="75%" width="100%" alt="employee user login"/>
</p>
<br />
<br />
<p>
  I hope this tutorial helped you learn a little bit about network security protocols and observe traffic between virtual machines. This can be easily done on a PC or a Mac. Mac would just have an extra step to download the Remote Desktop App.
</p>
<p>
  Now that we're done, DON'T FORGET TO CLEAN UP YOUR AZURE ENVIRONMENT so that you don't incur unnecessary charges.
</p>
<p>
  Close your Remote Desktop connection, delete the Resource Group(s) created at the beginning of this tutorial, and verify Resource Group deletion.
</p>
