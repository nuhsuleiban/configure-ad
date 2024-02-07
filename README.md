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


<h2>Deployment and Configuration Steps</h2>

n this lab we will create two VMs in the same VNET. One will be a Domain Controller, the other will be a Client machine. We will change the DC to a static IP because its offering Active Directory services to the client machine. The client machine will be joined to the domain. We will configure the DNS settings on the client machine, the client machine will use the DC as its DNS server.

<img width="1271" alt="image" src="https://github.com/nuhsuleiban/configure-ad/assets/153963865/9212c34d-29c3-43da-be3b-8bd1cd1f1f72">

DC-1 has to have a static Private IP Address. Client one will connect to DC-1 to ensure connectivity we will try to ping DC-1 from Client-1. At first the ping will not work correctly. We have to enable ICMPv4 on the firewall on DC-1. Now we can ping DC-1 successfully from Client-1.

<img width="1271" alt="image" src="https://github.com/nuhsuleiban/configure-ad/assets/153963865/076ae9fb-e471-4cfe-b356-34d2a9e51b88">

<img width="1271" alt="image" src="https://github.com/nuhsuleiban/configure-ad/assets/153963865/6da46046-f790-460b-893f-e6e89a7d5d82">

Now we will log back into DC-1 to install AD Users & Computers. Promote the VM to DC, setup a new forest as "mydomain.com" afterwards restart then log back into DC-1 as user: "mydomain.com\labuser". If executed properly, you should be able to run AD Users & Computers as shown below.

<img width="1271" alt="image" src="https://github.com/nuhsuleiban/configure-ad/assets/153963865/d0ac0d0f-d475-44b3-a9bf-f891770ea215">

We can start creating Organizational Units (OU). Let's first create an OU named _EMPLOYEES. Create another OU named _ADMINS. In order to do that, right click on the domain area. Select new->Organizational Unit and fill out the field. Then click inside of your OU and right click, select new and select user and fill out the information for your new user. The user should be named Jane Doe, she is going to be an Admin so her username will be Jane_admin. Lastly add Jane to the domain admins security group.

<img width="1271" alt="image" src="https://github.com/nuhsuleiban/configure-ad/assets/153963865/09941b4c-8a14-4474-9579-98e28bfef178">

<img width="1271" alt="image" src="https://github.com/nuhsuleiban/configure-ad/assets/153963865/7312fa2f-2e8e-484b-9284-5f6b1050be43">

From now on you can use Jane_admin as the administrator account. Now we will join Client-1 to the domain (mydomain.com). From the azure portal we will change client-1's DNS settings to the DC's Private IP address. After you do that restart Client-1 from within the Azure portal. Our picture below shows verification that client-1 is using DC-1 as its DNS.

<img width="1271" alt="image" src="https://github.com/nuhsuleiban/configure-ad/assets/153963865/fdf640c3-f93e-4281-9c1d-9b0ae27d6517">

<img width="1271" alt="image" src="https://github.com/nuhsuleiban/configure-ad/assets/153963865/ff95409f-1084-4367-978b-c31e1cce61f0">

We have to join Client-1 to the domain. Navigate to your system settings and go to "About." Select Rename this PC (advanced). From there select to change the domain. Enter "mydomain.com." Enter your credentials from mydomain.com\labuser. Your computer will restart and then client-1 will be a part of mydomain.com.

<img width="1271" alt="image" src="https://github.com/nuhsuleiban/configure-ad/assets/153963865/239c9bd3-3456-4ec5-bea5-4d997a3b9db5">

Client-1 is now a part of the domain. Now we will set up remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open System Properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. After completing those steps you should be able to log into Client-1 as a normal user.

<img width="1271" alt="image" src="https://github.com/nuhsuleiban/configure-ad/assets/153963865/e85f356e-f665-427b-9d30-a04284a1e8ea">

Lastly to verify that normal users can RDP into Client-1, we will use a script to generate thousands of users into the domain. We will input the script in powershell. After the users are created we will select one and RDP into Client-1.

<img width="1271" alt="image" src="https://github.com/nuhsuleiban/configure-ad/assets/153963865/6dcb69b5-9add-432f-be1f-78adf99b9b64">
<img width="1271" alt="image" src="https://github.com/nuhsuleiban/configure-ad/assets/153963865/9bce1683-c816-4511-812f-8693d2f58199">
<img width="1271" alt="image" src="https://github.com/nuhsuleiban/configure-ad/assets/153963865/7ed8b35a-4262-4304-bfdc-4cc4ee5e4bd8">

As you can see the Powershell script created a user with the username "bab.hubo" We were able to login to Client-1 with his credentials as a normal user.



