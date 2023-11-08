<h1>Active Directory - Home Lab</h1>

 

<h2>Description</h2>
In this lab I will walk you through how to create an Active Directory home lab environment using Oracle Virtual Box. Through the configuration and operating of this lab we will understand how Active Directory and Windows networking works.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Oracle VirtualBox</b>

<h2>Environments Used </h2>

- <b>Windows 10</b>
- <b>Windows Server 2016</b>

<h2>Walk-through:</h2>

<h2>Setting up the Virtual Machine:</h2>
<p align="center">
Creating a new Virtual Machine. Add 4096 MB, 2 processors and 50gb of virtual hard disk space then finish: <br/>
<img src="https://i.imgur.com/R8ZSYZU.png" height="60%" width="60%" alt=""/>
<br />
<br />
Once finished go to settings, general > advanced and enable bidirectional. Then go to Network > Adapter 2 and attach to internal network: <br/>
<img src="https://i.imgur.com/leWPPv2.png" height="60%" width="60%" alt=""/> <img src="https://i.imgur.com/e5cRrIS.png" height="60%" width="60%" alt=""/>
<br />
<br />
Start virtual machine and go through setup. Click custom, select the drive then wait for installation. Select Desktop Experience: <br/>
<img src="https://i.imgur.com/aUPvSL0.png" height="60%" width="60%" alt=""/>
<br />
<br />
Make the password whatever you want, in this lab we chose 'Password1':  <br/>
<img src="https://i.imgur.com/ucijN89.png" height="60%" width="60%" alt=""/>
<br />
<br />
<h2>Configuring the NIC:</h2>
<p align="center">
Once logged in. Navigate to the Network settings > change adapter options:<br/>
<img src="https://i.imgur.com/nZxKNOc.png" height="60%" width="60%" alt=""/>
<br />
<br />
Configure the internal NIC by finding which adapter contains the IP: 169.254.174.68. We do this by right click > status > details:<br/>
<img src="https://i.imgur.com/ekW7ATd.png" height="60%" width="60%" alt=""/>
<br />
<br />
Go to the status of our internal NIC > properties > 'Internal Protocol Version 4 (TCP/IPv4) > properties:<br/>
<img src="https://i.imgur.com/d5AAg28.png" height="60%" width="60%" alt=""/>
<br />
<br />
Configure the IPv4 settings with the following:<br/>
<img src="https://i.imgur.com/aWLEDmX.png" height="60%" width="60%" alt=""/>
<br />
<br />
Rename our external NIC to _INTERNET and the internal NIC to _INTERNAL:<br/>
<img src="https://i.imgur.com/YTibKye.png" height="60%" width="60%" alt=""/> 
<h2>Installing Active Directory Domain Services and setting up our domain: </h2>
<p align="center">
Change our computer name by going to system > change settings > change > type 'DC' then restart: <br/>
<img src="https://i.imgur.com/Auv6Paf.png" height="60%" width="60%" alt=""/>
<br />
<br />
To install Active Directory Domain Services go to Server Manager > Add roles and features > Role-Based or feature-based installation select a server from the server pool 'DC' > install: <br/>
<img src="https://i.imgur.com/tXccNIh.png" height="60%" width="60%"/> <img src="https://i.imgur.com/ON36ZVM.png" height="60%" width="60%"/> <img src="https://i.imgur.com/Oz4QrM6.png" height="60%" width="60%"/>
<br />
<br />
Click the yellow triangle in server manager > promote this server to a domain controller: <br/>
<img src="https://i.imgur.com/OXziVWf.png" height="60%" width="60%" alt=""/>
<br />
<br />
In Deployment Configuration > Add a new forest > mydomain.com > enter password > uncheck 'Create DNS delegation' and click next > Install. The computer will then restart: <br/>
<img src="https://i.imgur.com/XkS7CRy.png" height="60%" width="60%" alt=""/>
<br />
<br />
<h2>Installing RAS/NAT:</h2>
<p align="center"> 
In Server Manager dashboard, we will go back to add roles and features and repeat the process we did before when we installed Active Directory. Click 'Role-based or feature-based installation > Select our server from the server pool > click 'Remote Access' > check 'Routing' and add features > then Install <br/>
<img src="https://i.imgur.com/pCG651t.png" height="60%" width="60%" alt=""/>   <img src="https://i.imgur.com/5LhE3Sc.png" height="60%" width="60%" /> <img src="https://i.imgur.com/8ZbC1YU.png" height="60%" width="60%" /> <img src="https://i.imgur.com/ny5rDK5.png" height="60%" width="60%" />
<br />
<br />
In server manager go to Tools > Routing and Remote Access:<br/>
<img src="https://i.imgur.com/wNf1nkg.png" height="60%" width="60%" alt=""/>
<br />
<br />
Right click on DC (local) > Configure and enable routing and remote access to launch the wizard:<br/>
<img src="https://i.imgur.com/SPcoGoA.png" height="60%" width="60%" alt=""/>
<br />
<br />
Select Network Address Translation NAT:<br/>
<img src="https://i.imgur.com/r9MMa4r.png" height="60%" width="60%" alt=""/>
<br />
<br />
NOTE: If you dont see our networks that we configured, restart the routing and remote access wizard:<br/>
<img src="https://i.imgur.com/MWo0qWy.png" height="60%" width="60%" alt=""/>
<br />
<br />
Use this public interface to connect to the internet > click on our external NIC _INTERNET then next:<br/>
<img src="https://i.imgur.com/i8ucq6T.png" height="60%" width="60%" alt=""/>
<br />
<br /> 
Finish the wizard. Then we will see the green arrow on our DC (local) we have successfully configured RAS/NAT:<br/>
<img src="https://i.imgur.com/Mn2g1KF.png" height="60%" width="60%" alt=""/>
<br />
<br />
<h2>Installing DHCP:</h2>
<p align="center">  
Just like we did before, we will go to Server Manager Dashboard > Add roles and features > Role-based or feature-based installation > Select our server > Select DHCP and add features > then install:<br/>
<img src="https://i.imgur.com/9iNWRJz.png" height="60%" width="60%" /> <img src="https://i.imgur.com/yzq2otU.png" height="60%" width="60%" />
<br />
<br />
In Server Manager we go to tools > DHCP. It should look like the following:<br/>
<img src="https://i.imgur.com/yAv13LZ.png" height="60%" width="60%" alt=""/>
<br />
<br />
Underneath our dc.mydomain.com. Right click on IPv4 > select 'New scope':<br/>
<img src="https://i.imgur.com/k9J4SpT.png" height="60%" width="60%" alt=""/>
<br />
<br />
We will make our scope name the following:<br/>
<img src="https://i.imgur.com/cmKyQjb.png" height="60%" width="60%" alt=""/>
<br />
<br />
For our range we will include the following:<br/>
<img src="https://i.imgur.com/ZTlC7am.png" height="60%" width="60%" alt=""/>
<br />
<br />
We will leave the 'Add exclusions and Delay' section blank:<br/>
<img src="https://i.imgur.com/7Hxpy82.png" height="60%" width="60%" alt=""/>
<br />
<br />
Leave the lease duration at 8 days:<br/>
<img src="https://i.imgur.com/DFAEknI.png" height="60%" width="60%" alt=""/>
<br />
<br />
Configure the options now:<br/>
<img src="https://i.imgur.com/Vq4i74f.png" height="60%" width="60%" alt=""/>
<br />
<br />
For the router default gateway insert the following, make sure to hit 'add':<br/>
<img src="https://i.imgur.com/E6xvLQU.png" height="60%" width="60%" alt=""/>
<br />
<br />
Our Domain Controller is our DNS:<br/>
<img src="https://i.imgur.com/R2UPefv.png" height="60%" width="60%" alt=""/>
<br />
<br />
Skip WINS:<br/>
<img src="https://i.imgur.com/Gaq2Jw9.png" height="60%" width="60%" alt=""/>
<br />
<br />
Active the scope now, then finish the wizard:<br/>
<img src="https://i.imgur.com/eqLVrhe.png" height="60%" width="60%" alt=""/>
<br />
<br />
Right click dc.mydomain.com and 'authorize' then right click again and 'refresh' We should now have green arrows on our IPv4. DHCP is successfully configured:<br/>
<img src="https://i.imgur.com/KXZj8M1.png" height="60%" width="60%" alt=""/>
<br />
<br />
<h2>Creating a Dedicated Admin Account:</h2>
<p align="center"> 
Go to start > Windows Administrative Tools > Active Directory Users and Computers:<br/>
<img src="https://i.imgur.com/lkFvAfL.png" height="60%" width="60%" alt=""/>
<br />
<br />
Right click mydomain.com > New > Organizational Unit. Insert the following:<br/>
<img src="https://i.imgur.com/P9LXDnY.png" height="60%" width="60%" alt=""/>
<br />
<br />
Right click on the new _ADMINS folder we created > New > User. Fill out the boxes with your information. For user logon name, include 'a-firstnameinitiallastname':<br/>
<img src="https://i.imgur.com/WjDVFie.png" height="60%" width="60%" alt=""/>
<br />
<br />
Include the password with 'Password never expires':<br/>
<img src="https://i.imgur.com/HwfwHx4.png" height="60%" width="60%" alt=""/>
<br />
<br />
Once done you should get the following confirmation:<br/>
<img src="https://i.imgur.com/U7lkHkT.png" height="60%" width="60%" alt=""/>
<br />
<br />
Right click on the new user we created in the _ADMINS folder > Properties:<br/>
<img src="https://i.imgur.com/ZjeAWxp.png" height="60%" width="60%" alt=""/>
<br />
<br />
In Properties go to Member of, click Add. In the box, enter in 'Domain Admins' then 'Check Names' it should be underlined. Then click Ok:<br/>
<img src="https://i.imgur.com/Egp2yaM.png" height="60%" width="60%" alt=""/>
<br />
<br />
Log in with the new account:<br/>
<img src="https://i.imgur.com/bQ8dcdb.png" height="60%" width="60%" alt=""/>
<br />
<br />
<h2>Executing the Powershell script to make 1000 users in Active Directory:</h2>
<p align="center">
Insert text here:<br/>
<img src="insert URL" height="60%" width="60%" alt=""/>
<br />
<br />
Insert text here:<br/>
<img src="insert URL" height="60%" width="60%" alt=""/>
<br />
<br />
Insert text here:<br/>
<img src="insert URL" height="60%" width="60%" alt=""/>
<br />
<br />
Insert text here:<br/>
<img src="insert URL" height="60%" width="60%" alt=""/>
<br />
<br />
Insert text here:<br/>
<img src="insert URL" height="60%" width="60%" alt=""/>
<br />
<br />














</p>
