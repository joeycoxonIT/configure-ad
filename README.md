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
<br />

<h3>14) Setup Remote Desktop for non-administrative users on Client 1</h3>

<p>

- Log into your "client-1" virtual machine as "Jane Doe" (mydomain.com\jane_admin)
- Right click the Start menu at the bottom left of the screen in your "client-1" virtual machine, then click "System"
- Once you're at the "System" page, click "Remote desktop"
- Under "User accounts", click "Select users that can remotely access this PC.
</p>
<p>
<img src="https://i.imgur.com/pwonAmX.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Once a tab called "Remote Desktop Users" opens, click "Add"
- In the empty field, type "domain users" to allow all domain users to log into the virtual machine. Then click "OK".
</p>
<p>
<img src="https://i.imgur.com/jqMXoXf.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>15) Create Additional Users in DC-1 VM</h3>
<p>

- Log into your "dc-1" virtual machine as "Jane Doe"
- In the search bar at the bottom of the screen, search for and open "Windows Powershell ISE". Right click it and run it as an administrator
- Create a new File (click "File" -> "New" at the top left of the page) and copy and paste the contents of this script: "https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1" into it.
- Save the file. For this tutorial, we will save it to our Desktop and name it "create-users"
</p>
<p>
<img src="https://i.imgur.com/z0kadKU.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Run the script by clicking the green play button at the top of the page
</p>
<p>
<img src="https://i.imgur.com/mzlL4oE.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Observe the users being created
</p>
<p>
<img src="https://i.imgur.com/QhLPpKv.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>16) Verify that users in Active Directory have successfully been created</h3>
<p>

- Open Active Directory Users and Computers
- Click the "_EMPLOYEES" folder to verify that users have been created
</p>
<p>
<img src="https://i.imgur.com/ZrEHcee.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>17) Log into Client 1 as one of the newly created users</h3>
<p>

- Choose any newly created user from the _EMPLOYEES folder in Active Directory and use it to log into your "client-1" virutal machine
  - The format of the username should be the same as your admin user (Example: mydomain.com\bot.jep)
  - NOTE: The password can be found at the top of the script. It is the same for all users.
</p>
<br />

<h3>18) Configuring Account Lockout Policy</h3>

<h4>Scenario:</h4>
<p>
In some cases, unauthorized users will have access to accounts that they shouldn't. To prevent this, organizations will often create a lockout policy to ensure that only authorized users have access these accounts. This policy is often implemented by locking a user out of their account after a certain number of login attempts 
</p>
<p>


- Log in to your "dc-1" virtual machine
- Right click the Start menu. Click "Run", type "gpmc.msc" to open "Group Policy Management", then click "OK".
</p>
<p>
<img src="https://i.imgur.com/msRKYUY.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Now that you opened "Group Policy Management", Expand "Forest: mydomain.com" -> "Domains" -> "mydomain.com" until you see "Default Domains Policy".
- Right-click "Default Domains Policy" and click "Edit"
</p>
<p>
<img src="https://i.imgur.com/6bt7AKq.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Once you're at the editor, expand "Policies" -> "Windows Settings" -> "Security Settings" -> "Account Policies". Then click "Account Lockout Policy"
</p>
<p>
<img src="https://i.imgur.com/E8YUbPy.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>

- You will see three primary settings: "Account lockout duration", "Account lockout threshold", and "Reset lockout account after"
  - Click "Account lockout duration". For this tutorial, we will set the duration to "30 minutes". Once it's set, click "Apply" then "OK".
  - Click "Account lockout threshold". Set the "invalid logon attempts" to "5". Click "Apply" then "OK".
  - Click "Reset account lockout counter after". Set it to "30 minutes". Click "Apply" then "OK"
    - NOTE: Make sure for all of these that "Define this policy setting" is checked.
</p>
<p>
<img src="https://i.imgur.com/Cd1PpYB.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/YRUuDkZ.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/23rQgbU.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>19) Dealing with Locked User Accounts</h3>

<h4>Scenario:</h4>
<p>
A user is locked out of their account due to too many failed login attempts. The user seeks help from the domain user to unlock their account.
</p>
<br />

<p>
  
- Simulate an account lockout by attempting to login as one of the users you created earlier with a wrong password more than five times. If done correctly, the notification below will pop up.
</p>
<p>
<img src="https://i.imgur.com/EGTK4ja.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Now go back into your "dc-1" virtual machine. Open "Active Directory Users and Computers"
- Right click "mydomain.com" and select "Find"
- In the empty field under "Name", type in the username of the account that was locked out. Then click "Find now"
- Now that you're at the "Properties" page for this account, click the "Account" tab. Make sure "Unlock account" is checked. Then click "Apply" and "OK".
</p>
<p>
<img src="https://i.imgur.com/CJPdY3y.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Now attempt to log into your "client-1" virtual machine with the account you just unlocked to verify that the above steps worked.
</p>
<br />

<h3>20) Enabling and Disabling Accounts</h3>

<h4>Scenario:</h4>
<p>
A user believes that their account information has been leaked and an unauthorized user has access to it. The user seeks help from the domain administrator to disable the account.
</p>
<br />

<p>

- Login to your "dc-1" virtual machine as "jane_admin"
- Open "Active Directory Users and Computers"
- Right-click "mydomain.com" and select "Find"
- In the empty field under "Name", type in the username of the account that wants their account disabled. Then click "Find now"
- Right click the found user and click "Disable Account"
</p>
<p>
<img src="https://i.imgur.com/2mu121O.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Verify that the account has been disabled by attempting to log into your "client-1" virtual machine with it. If it's disabled you will get the message displayed below.
</p>
<p>
<img src="https://i.imgur.com/uyQtbDg.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Now that the account is locked, we will observe the logs for suspicious activity
- In the search bar at the bottom, search for "eventvwr.msc"
- Once it's open, expand "Windows Logs" then click "Security"
- Observe the logs and you will find some "Audit Failures"
  - If you look at the details for some of the logs, you will find that these particular audit failures are failed login attempts from when we attempted to login with user's account from earlier 
</p>
<p>
<img src="https://i.imgur.com/QU97DCy.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p></p>
<br />

<h3>Congratulations! That's the end of this tutorial.</h3>
