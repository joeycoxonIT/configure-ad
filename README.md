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
