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

- Create and configure a Domain controller virtual machine
- Create client virtual machine
- Establish a connection between the Domain Controller and Client virutal machines
- Ensure connectivity between two virtual machines by inspecting network traffic
- Install Active Directory
- Create an Active Directory Forest
- Create a Domain Admin User
- Join Client-1 to the Domain
- Set up Remote Desktop for non-administrative users
- Create additional users

<h2>Deployment and Configuration Steps</h2>

<h3>1) Create a Resource Group and Virtual Network</h3>
<p>

- Navigate and create a resource group. For this tutorial, we will name it "Active-Directory-Lab"
- Create a virtual network and select the resource group you just created
</p>
<p>
<img src="https://i.imgur.com/bMJ2b3V.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/M7grpJF.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/crWNjFm.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>2) Create a Domain Controller</h3>

<p>

- Create a virtual machine and name it "dc-1"
- Under "Image" select "Windows Server 2022 Datacenter: Azure edition
- Under "Size" select an option with at least 2 vcpus
- Create a username and password
- Click "Next: Networking" then under "Virtual network" select the virtual network you previously created
- Make sure "Licensing" is checked, then click "Review + Create"
</p>
<p>
<img src="https://i.imgur.com/NiJFyHY.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/0p05Kz7.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>3) Create a "Client" virtual machine</h3>
<p>

- Create a virtual machine and name it "client-1"
- Under "Image" select "Windows 10 Pro"
- Under "Size" select an option with at least 2 vcpus
- Create a username and password
- Click "Next: Networking" then under "Virtual network" select the virtual network you previously created
- Make sure "Licensing is checked, then click "Review + Create"
</p>
<p>
<img src="https://i.imgur.com/foM24y2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/crWNjFm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>4) Set the Domain Controller's Private IP Address to "Static"</h3>

<p>

- Navigate to your Domain Controller in your virtual machines page("dc-1")
- Select "Network settings"
- Select "Network Interface/IP configuration"
- Select "ipconfig1"
- Under "Allocation" select "Static"
- Click "Save" so that the Private IP address remains unchanged
</p>
<p>
<img src="https://i.imgur.com/QMvsj4R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/C92zjcW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>5) Connect to the Domain Controller</h3>

<p>

- Go to your virtual machines page and copy the public IP address under "dc-1"
- In the search bar at the bottom of the screen, type and select "Remote Desktop Connection"
- Enter your credentials and log into your VM
</p>
<p>
<img src="https://i.imgur.com/C6Q3tkl.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/SkvydG0.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>6) Disable Firewalls in the Domain Controller</h3>

<p>

- In your domain controller, right click the "Windows" icon at the bottom left of the screen, then click run and enter "wf.msc" to find "Windows Defender Firewall with Advanced Security"
- Once you're there, then click "Windows Defender Firewall Properties"
- Then under "Domain Profile", "Private Profile", and "Public Profile", set the "Firewall State" to off. then click "Apply" and "OK"
  - NOTE: Under normal circumstances, you would leave the firewalls on, however, for the sake of this tutorial, we will turn them off to prevent any complications.
</p>
<p>
<img src="https://i.imgur.com/y14ZFPd.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/zwXVyNy.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/8ThrKSt.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>

<h3>7) Connect the Client VM to the Domain Controller VM</h3>

<p>

- Back in Azure, in your Virtual machines page, nagivate to your "dc-1" virtual machine, and copy the private IP address
- Once you have the private IP address from your "dc-1" virtual machine, go to your "client-1" virtual machine, then click "Network Settings" -> the "Network Interface / IP Configuration" box -> "Settings" -> "DNS Servers"
- Under "DNS Servers" select "Custom" then paste the private IP address and click "Save"
- Navigate back to your Virtual machines page, then restart the "client-1" virtual machine
</p>
<p>
<img src="https://i.imgur.com/Q5EONqK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/6D6OUT1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>8) Connect to Client VM with Remote Desktop</h3>

<p>

- Copy the public IP address of your "client-1" virtual machine
- Go to the search bar at the bottom of the screen and search for "Remote Desktop Connection"
- Paste the public IP address and connect to your virtual machine
</p>
<br />

<h3>9) Ensure connectivity between the Domain Controller and the Client</h3>

<p>

- In your "client-1" virtual machine, go to the search bar at the bottom of the screen and search for "Powershell" and open it
- Attempt to ping the private IP address of your "dc-1" virtual machine (example: ping 10.0.0.4)
- Observe and make sure the ping is successful
</p>
<p>
<img src="https://i.imgur.com/7yTArsn.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>10) Install Active Directory</h3>

<p>

- In your "dc-1" virtual machine, open "Server Manager Application"
- Click "Add Roles and Features"
- Click "Next" until you see "Server Roles"
- Make sure "Active Directory Domain Services" is checked, then click "Add Features"
- Continue to click "Next" until you're at the "Confirmation" page
- Make sure the box that says "Restart the destination server automatically if required" is checked, then click "Install"
</p>
<p>
<img src="https://i.imgur.com/1Y0uvdN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>11) Promote DC-1 to a Domain Controller</h3>

<p>

- In your Server Manager, click the flag icon at the top right of the page, then click "Promote this server to a domain controller"
- Select "Add a new Forest" and enter a domain name under "Root domain name". For this tutorial, we will name it "mydomain.com".
- Click Next, then create a password
- Contiunue to click "Next" until you see "DNS options". Make sure "Create DNS delegation" is unchecked
- Continue to click "Next" until you're at the Prerequisites page. Wait for the prerequisites to be verified, then click "Install".
- Log out of your "dc-1" virtual machine and log back into it as "mydomain.com/(username)"
</p>
<p>
<img src="https://i.imgur.com/nxrRtSD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/aK066DJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>12) Create a Domain Admin user within the domain</h3>

<p>

- In your "dc-1" VM, go to the search bar at the bottom of the screen and search for "Active Directory Users and Computers". Open it once you see it.
- Right click "mydomain.com" then click "New" -> "Organizational Unit"
- Name the organizational unit "_EMPLOYEES" then click "OK"
- Create another organizational unit and name it "_ADMINS"
- To create a new admin, right click on the "_ADMINS" folder then click "New -> "User"
- Enter the credentials for the new user. For this tutorial, we will name them "Jane Doe". Once that's done, click "Finish"
- Next we will add Jane Doe to the "Domain Admins" Security Group. Right click on Jane Doe and click "Properties" -> "Member of"
- In the "Member of" tab, click "Add"
- Type "domain admins" in the empty field, then click "Check Names" to confirm that you found the correct name. Then Click "Apply" and "OK".
- Log out of your "dc-1" VM, then log back in as "Jane Doe" (mydomain.com\jane_admin). From now on, this will be used as the admin account.
</p>
<p>
<img src="https://i.imgur.com/y5V6Hbx.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/HTy2ZA0.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/7vGHRlG.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/EvS2fRA.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/023fN8q.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/3wDmYl0.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>13) Join Client 1 to the Domain</h3>

<p>

- Log into the "client-1" virtual machine as the original local admin (labuser)
- Right click the Windows start button in the bottom left of the screen, then open "System"
- Click "Rename this PC advanced" at the right of the page
- In the "Computer Name" tab click "Change"
- Under "Member of" select "Domain" and type "mydomain.com" in the field. Then click "OK".
- Enter the username and password with the "Jane Doe" information. Then click "OK".
- If done correctly, you will see the following window pop up.
- Click "OK" and your VM will restart
- Within your "dc-1" virtual machine, go to the search bar at the bottom of of the screen and search for and open "Active Directory Users and Computers"
- Open the "Computers" folder to verify that "client-1" is there
</p>
<p>
<img src="https://i.imgur.com/32uw3ue.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/q1Xofv7.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/hG13380.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/VL5QJNw.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/3rOP6q9.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
