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

- Setup Domain Controller in Azure
- Setup Client-1 in Azure
- Install Active Directory on Domain Controller via Remote desktop
- Create a Domain Admin user within the domain
- Join Client-1 to your domain 
<h2>Deployment and Configuration Steps</h2>

<p>
  
![image](https://github.com/user-attachments/assets/7883b64b-c2ec-462b-9abb-3e97ead1297f)

</p>
<p>
1.) Create a resource group witin Azure where the virtual network and virtual machines will be assigned.
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/dc527945-d37e-404b-b76a-6ae6af922c16)

</p>
<p>
2.) Create and assign a virtual network to the resource group
<br />

<p>
  
![image](https://github.com/user-attachments/assets/dcc6ad07-af3c-417a-93e4-9971ea652bf5)

</p>
<p>
3.) Create DC and Client VMs and assign them to the same resource group (Active-Directory-Lab) and virtual network (Active-Directory-Vnet)
</p>
<br />

![image](https://github.com/user-attachments/assets/ab5a8563-b52f-4528-882c-bc1924c08dc7)


</p>
<p>
4.) Set DC-1 NIC private ip address to static (none-changing) so that the client VM can use DC-1 as a DNS server once configured to do so.
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/17e34535-faa5-40e0-8e7b-43b7a0edf25e)

</p>
<p>
5.) Disable the firewall in DC-1 for testing connectivity
<br />

<p>
  
![image](https://github.com/user-attachments/assets/baf96cd7-bf80-47ee-b2fe-71feca667aae)


</p>
<p>
6.) Within Azure, go to the client VM and set Client-1’s DNS settings to DC-1’s Private IP address. This will set the DC-1 VM (our DNS server) as client-1's VM DNS server.
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/e8e2f4b0-f661-4631-babd-c0a2eecdea6e) ![image](https://github.com/user-attachments/assets/f55d2a0f-8b3e-4738-9c55-2117122b9885) ![image](https://github.com/user-attachments/assets/a6072010-d890-487f-9d34-ac89d2fcc4e2)



</p>
<p>
7.) Login to Client-1 and attempt to ping DC-1’s private IP address using Powershell. Ensure the ping succeeded, then from Client-1, open PowerShell and run ipconfig /all. The output for the DNS settings should show DC-1’s private IP Address
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/c0004b03-75fd-4e1c-bbee-22091c04b631) ![image](https://github.com/user-attachments/assets/f15b6596-1be8-402d-b992-f9a6dfe9ff45)


</p>
<p>
8.) In DC-1, open server manager and navigate to "Add roles and features". Next, select and install "Active Directory Domain Services"
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/59307d88-a108-4215-b967-b77874875df0) ![image](https://github.com/user-attachments/assets/68e9cc84-1c6c-48aa-8fdf-a7d34040af09) ![image](https://github.com/user-attachments/assets/843aa33d-c4ae-43e2-b035-98971a242431)



</p>
<p>
9.) In the top right corner of the server manager page, click the flag and select "Promote this server to a domain controller".  Next, add a new forest and enter a domain name. Once this done, the DC-1 VM will restart
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/342df4a2-ab8a-4b52-88f2-1d728411f868)

</p>
<p>
10.) Restart and then log back into DC-1 as user: mydomain.com\labuser
<br />

<p>

![image](https://github.com/user-attachments/assets/d6a879a1-325d-483e-98d6-b00effd6235b)

</p>
<p>
11.) In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Create a new OU named “_ADMINS by right-clicking your domain name -> New -> Organizational Unit
</p>
<br />

<p>

  ![image](https://github.com/user-attachments/assets/e47903dd-c5ca-4943-8913-464f5b6efb7e) ![image](https://github.com/user-attachments/assets/5b0e32f4-d2f1-4fd6-9429-45d1fc13dc34)


</p>
<p>
12.) In Active Directory Users and Computers (ADUC), create a neweployee  within the "_ADMINS" by right-clicking the _ADMINS foler -> New -> User. From there you can assign their login credentials. (For the purposes of this lab I changed a few password settings)
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/2d613f39-48e4-45d9-941f-46ebbb809a33) ![image](https://github.com/user-attachments/assets/4d1b3df4-631f-4bcf-bc91-56a60594a95a) ![image](https://github.com/user-attachments/assets/ff5f5003-6181-4017-8948-905c7d34028f)



</p>
<p>
13.) Add the user to the “Domain Admins” Security Group. In Active Directory Users and Computers (ADUC), on the left hand side select the _ADMINS folder and right click the employee you created -> properties -> member of tab -> and add them to the built in admin users group
<br />

<p>
  
![image](https://github.com/user-attachments/assets/3dec2632-bc4d-445f-8fa9-b5eb50bdbb9f)

</p>
<p>
14.) Log out of the DC-1 VM and log back in as the admin account created before. This will be the admin account for the rest of the project
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/40134927-fca5-4f58-8c72-b83521a258ea)
 ![image](https://github.com/user-attachments/assets/06330dfd-8419-450f-991e-3a327f304382) ![image](https://github.com/user-attachments/assets/06d58d78-edd5-42bc-81fd-c6dce0865163)


</p>
<p>
15.) Login to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart). Open system settings -> rename this PC(advanced) -> Select "Chnage" -> Enter your domain name -> enter the admin user crdentials created from before and allow your VM to restart
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/1d0719a7-dcad-4df2-92b6-48e364153865) ![image](https://github.com/user-attachments/assets/917975cc-9ee7-4cea-8222-f2608e5b91a2)


</p>
<p>
16.) Login to the Domain Controller and verify Client-1 shows up in ADUC and create a new OU named “_CLIENTS” and drag Client-1 into there
</p>
<br />
