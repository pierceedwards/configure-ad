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

<h2>🖥️ Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Set up Azure Virtual Machines
- Configure Acitve Directory Domain Services
- Promote Server to Domain Controller
- Connect and Manage Domain-Joined Client(s)

<h2>Deployment and Configuration Steps</h2>

Step 1: Set Up Azure VM's
-Create two VM's (Domain Controller and Client) in Azure
-Configure network settings and security groups


![Screenshot 2025-03-24 122816](https://github.com/user-attachments/assets/09dbeab3-4113-411e-a8b2-a6d33f0a07fb)

DC-1 (Domain Controller): Running Windows Server 2022
Client-1: Running Windows 10
Ensure both Virtual Machines are in the same Virtual Network

![Screenshot 2025-03-24 122316](https://github.com/user-attachments/assets/364fe9d0-1467-4902-a8bd-830ab5bf98cc)


After creating the virtual machines , set Client-1's DNS settings to DC-1 Private IP address via the NIC. In order to change the DNS server for client-1 we need to go to our virtual machines and click on client-1. From here we: click on Network settings -> DNS servers (under settings option) -> custom -> enter private address of DC-1 (for this example). This will reroute traffic to DC-1 instead of the default gateway given by azure. 

![Screenshot 2025-03-24 122905](https://github.com/user-attachments/assets/d6d26e15-994e-4dda-b7d3-78f752ced38c)


To confirm this we can log into client-1 and execute the command [ipconfig /all] and [ping 10.0.0.4] on powershell (in this example we have set Mainframe to 10.0.0.4) in order to confirm the DNS configuration and connection.

![Screenshot 2025-03-25 142958](https://github.com/user-attachments/assets/2fe0020e-5952-4722-b58d-edc5758016d1)

 to make DC-1 into a domain controller, we need to go to server manager and click on add roles and services. From here we can click next until we reach the page (server roles) and select "Active Directory Domain Services". We can then click next until given to the option to install.

![Screenshot 2025-03-24 131224](https://github.com/user-attachments/assets/553dc966-d216-4d6b-a96d-c53005add241)

![Screenshot 2025-03-24 131300](https://github.com/user-attachments/assets/7ef4998c-0d6d-44f3-bcd9-39c2faaa99fb)

![Screenshot 2025-03-24 131506](https://github.com/user-attachments/assets/9b7f57b6-ad5c-42d7-baaa-0af074a1abe2)

We now want to create a two folders, one for employees and another for admins, and create a domian admin as well. For the folders we can click the search bar and enter Active Directory Users and Computers and click on it -> right click on "mydomain.com" -> click new -> Organizational Unit -> enter file names ( in this case we create two, one for _EMPLOYEES and _ADMINS). 

![Screenshot 2025-03-24 132749](https://github.com/user-attachments/assets/da64616a-bb8e-4067-9daf-446eeee903e6)

![Screenshot 2025-03-24 132855](https://github.com/user-attachments/assets/237dd006-0800-429f-9f68-bf99fbfa3e0c)

![Screenshot 2025-03-24 132938](https://github.com/user-attachments/assets/89275399-ecf1-4c5b-8cee-354e8cc3a5b0)

Now in order to create a domain admin we can right click the _ADMINS folder -> New -> User. Fill out the information, in this case I created a domian admin named "Jane_admin", you will be clicked next then prompted for a password. Once completed, in order to make this user a domain admin we need to right click on the user ( in this case Jane Doe) -> properties -> Member of -> add -> enter "Domain Admins" -> check names -> ok -> apply then ok. This will add Jane doe as part of the Domain Admins group officially.

![Screenshot 2025-03-24 134842](https://github.com/user-attachments/assets/b34e8699-2167-4fd0-b3fe-286a35ad9afb)
![Screenshot 2025-03-24 133213](https://github.com/user-attachments/assets/2fa0bc8b-969d-4b6f-bf17-37b8a67ae9e1)




Next we need to add Client-1 to our domain. Go into settings -> Rename this PC (advanced) -> change -> click on Domain, enter domain ( in this case: mydomain.com) -> ok. This will add Client-1 to the mydomain.com but to make sure we will go back to the Domain controller. From here we can search for Active Directory Users and Computers -> click on Computers folder -> Client-1 should be present.

![Screenshot 2025-03-24 134259](https://github.com/user-attachments/assets/05805937-a262-4e13-8689-3453813a7469)
![Screenshot 2025-03-24 134913](https://github.com/user-attachments/assets/1bfdd722-3e30-4f83-bb6f-1d06a05784e2)

To Create users I used this script shown in the example. DC-1 as admin -> open Powershell ISE -> File -> New -> paste script into command line -> run script. If executed correctly the command line should generate users but to confirm we can head over to Active Directory Users and Computers -> _EMPLOYEES foler. There should be users in the folder _EMPLOYEES that is why it is important it is spelled correctly.



![Screenshot 2025-03-24 140617](https://github.com/user-attachments/assets/0d833920-9e32-4477-b71f-c724ab026276)







 
