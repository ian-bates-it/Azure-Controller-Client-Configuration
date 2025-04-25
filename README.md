# Configuring the Azure Controller and Client Virtual Machines

- After installing an Azure Windows 2022 Server to serve as the domain controller and an Azure Windows 10 Pro virtual machine to serve as the client, we need to make some configuration changes to both virtual machines.

- To <a href="https://github.com/ian-bates-it/Azure-Virtual-Machine-Setup">install an Azure Windows 2022 Server and Windows 10 Pro virtual machine, see this tutorial at this link.</a>

---


<p align="center">
<img src="https://github.com/user-attachments/assets/b3ea61fe-a76a-4b78-8cf4-9f34db958509" height="30%" width="30%"/>
</p>

---

---
<br />
<br />

<h2>Set the Windows 2022 Server Virtual Machine's NIC Private IP to Static</h2>

We will want our `Windows 2022 Server Datacenter: Azure Edition - x64 Gen2` to use a static IP address. 
- The Windows 2022 Server VM will serve as our Domain Controller.
- It is imperative that the Windows 2022 Server always has the same IP address. 

(Preparing AD Infra in Azure ~9:06)
When we create a VM in Azure it automatically assigns in a dynamic private IP address. 

We are going to tell our client machine (Windows 10 Pro) to use our controller (Windows 2022 Server) as the DNS Server. 
So we need to manually configure the DNS settings for our client to use the static Windows Server's IP address. (10.0.0.5)
If the private IP address for the Controller/Server changes, then our client can no longer use it as a DNS server. 



---
<br />

<h3>Step 1: Obtain the Private IP Address of our Windows Server</h3>

- To find the private IP address of our Windows Server, navigate to your Virtual Machines in Azure and follow the steps shown below.

1. Click on Virtual Machines
2. Click on the Windows 2022 Server.
3. Click on Networking
4. Click on Network Settings
5. View the Private IP Address listed for your Windows Server.
   - In my case, the private IP addresss of my Windows 2022 Server as `10.0.0.5` as shown below. 


<img src="https://github.com/user-attachments/assets/37ada912-3f9f-45c3-9989-a71826ddbba2" height="80%" width="80%" />



---
<br />

<h3>Step 2: Click on the Network interface / IP configuration button </h3>

- Click on the Network Interface / IP configuration button for your Windows Server as shown below.


<img src="https://github.com/user-attachments/assets/07256235-6606-46e1-a952-d217fca86050" height="80%" width="80%" />



---
<br />

<h3>Step 3: Click on ipconfig1 </h3>


- By default, our Windows Server is configured to have its private IP address assigned dynamically as shown below.
- Click on `ipconfig1` to change this setting as shown below.


<img src="https://github.com/user-attachments/assets/39a6bf51-a071-4b7a-a585-6b2c0873e608" height="80%" width="80%" />



---
<br />

<h3>Step 4: Edit IP Configuration </h3>

- In the right-hand column, change the Private IP address settings from `Dynamic` to `Static
- Confirm the Private IP Address matches the private IP for your Windows Server. Mine was `10.0.0.5` as shown below.
- Click the blue `Save` button at the bottom of the screen as shown below.

<img src="https://github.com/user-attachments/assets/acef0270-3381-4c67-b464-c06206cc42e1" height="60%" width="60%" />



---
<br />

<h3>Confirm your Windows Server is using a Static IP Address</h3>

- Azure will update the Windows Server network interface and then confirm the change has been made as shown below.

<img src="https://github.com/user-attachments/assets/c708bb36-9999-4622-967e-e37948911ff9" height="40%" width="40%" />

- Now your Windows Server will show its private IP address as being static as shown below.


<img src="https://github.com/user-attachments/assets/d9196407-4fef-4743-93d8-b227d82f68be" height="80%" width="80%" />


---
<br />


<h2>Disable the Firewall on Windows Server (For Testing Only)</h2>


(Preparing AD Infra in Azure ~13:27)
- For testing purposes only, we will disable the firewall on our Windows 2022 Server to make the communication to our client easier.
- Of course, you would never do this in live production.


<h3>Step 1: Remote into the Windows 2022 Server with Remote Desktop</h3>

- To remote into our Windows Server, we need to obtain the server's public IP address. 


In the Virtual Machine view in Azure, you can locate the public IP Address for your Windows 2022 Server as shown below: 

<img src="https://github.com/user-attachments/assets/048e7772-61a7-418f-a1dd-fbd31fc87fe4" height="80%" width="80%" />


---
<br />

<h3>Step 2: Remote into the Windows 2022 Server with Remote Desktop</h3>

- Open the Remote Desktop application on your local computer
- Run `mstsc.exe`
- Enter the Window Server's public IP address into the computer field as shown below and press connect.


<img src="https://github.com/user-attachments/assets/3c3cad80-e296-4e1e-9e78-85655f2026fb" height="40%" width="40%" />

- Enter in the Windows 2022 Server administrator username and password that you created when making the server in the Azure setup.
- Here I entered my Windows 2022 Server admin username (`Admin-DC`) and password and then pressed okay.

<img src="https://github.com/user-attachments/assets/34cb962f-6fea-450f-8638-7de2477a7976" height="40%" width="40%" />



---
<br />

<h3>Step 3: Open Windows Firewall on your Windows 2022 Server </h3>

- Run `wf.msc` to open the Windows Firewall on your Windows 2022 Server as shown below. 

<img src="https://github.com/user-attachments/assets/2475d2bd-b63c-49e9-a40a-f925915fcac3" height="40%" width="40%" />



---
<br />

<h3>Step 4: Click `Windows Defender Firewall Properties` </h3>

- As shown below, click on the `Windows Defender Firewall Properties`.


<img src="https://github.com/user-attachments/assets/347e4363-f9cd-4683-98c4-9b7a7d38c442" height="80%" width="80%" />



---
<br />

<h3>Step 5: Turn Firewall State to Off on Domain, Private and Public Profile Tabs </h3>

- Select the `Off` state for the `Domain Profile`, `Private Profile` and `Public Profile` as shown below.
- Then press `Ok`


<img src="https://github.com/user-attachments/assets/44f77b9e-61b9-4eb2-bb15-d687fcbc468c" height="80%" width="80%" />



---
<br />


<h2>Test Connectivity from Client (Windows 10 Pro VM) to Controller (Windows 2022 Server VM)</h2>

- Now that our Windows Server 2022 firewall is off, we want to test the connectivity from our Windows 10 Pro client to our Windows 2022 Server conroller.
- To do this, we will remote into our client Windows 10 Pro and ping the Windows 2022 Server controller private IP address.

---
<br />

<h3>Remote into our Windows 10 Pro client VM</h3>


- On your local machine, open up another Remote Desktop window.
- Run `mstsc.exe`
- Enter the Window 10 Pro public IP address into the computer field as shown below and press connect.
- In my example, the Windows 10 Pro public IP address was `20.81.44.196`

<img src="https://github.com/user-attachments/assets/eef10617-0667-402a-af36-bf132d53bbe2" height="80%" width="80%" />


- Enter the Administrator username and password for the Windows 10 Pro client that you created when we first set up the Windows 10 Pro virtual machine in Azure.
- For me, the username was `helpdesk`.
- Then click `Ok` to connect as shown below.

  <img src="https://github.com/user-attachments/assets/e32b0c5f-86d1-49a4-9d5c-853f8ba412ae" height="50%" width="50%" />


---

<h3>Ping the Windows Server from the Windows 10 Pro Client</h3>


- Open PowerShell on the Windows 10 Pro client.
- Run the commandL ping + <your-server-private-ip>
- So in my example, I ran the command: `ping 10.0.0.5`


<h4>Successful Ping Example</h4>
- If you receive a response back with zero packets lost, you have successfully 
- This confirms that our client Windows 10 Pro client can communicate with our Windows 2022 server controller. 
- Here is an example of the successful response I received. 

  <img src="https://github.com/user-attachments/assets/7ffa866d-0fd7-4dde-aaa0-8dc376d5bc96" height="50%" width="50%" />

<br />

<h4>Unsuccessful Ping Example</h4>
- If you receive 100% packet loss from your client ping to the controller the two virtual machines can not communicate with each other. 
- Here is an example of an unsuccessful ping. 

  <img src="https://github.com/user-attachments/assets/bc4fde14-25fa-4911-944e-0f4a5d8da81b" height="50%" width="50%" />




---
<br />


<h2>Update the Client Windows 10 Pro VM's NIC in the Azure Settings</h2>

In the Azure portal, navigate to the Virtual Machine Window. 

1. Select your Windows 10 Pro virtual machine
2. Click `Networking`
3. Click `Network Settings`
4. Click the Windows 10 Pro `Network interface / IP configuration` as shown below.

  <img src="https://github.com/user-attachments/assets/902790bd-2a40-43aa-8fc5-743a8409e0db" height="80%" width="80%" />


---
<br />

<h3>Select DNS Settings</h3>

- In the Windows 10 Pro Network Interface settings, select `DNS servers` from the left-hand menu as shown below.

  <img src="https://github.com/user-attachments/assets/4b3a1ccc-e97e-4727-89a2-0d1294495ea2" height="80%" width="80%" />
  


---
<br />

<h3>Set Windows 10 Pro DNS Server to use the Windows 2022 Server Private IP Address</h3>

1. Change the Windows 10 Pro DNS servers to `Custom`.
2. Enter your Windows 2022 Server's private IP address as the Windows 10 Pro DNS server.
   - In this example, my Windows 2022 Server's private IP address is `10.0.0.5`.
3. Then click the `Save` icon above as shown below.
  
  <img src="https://github.com/user-attachments/assets/f6390481-e94c-4f41-9eac-d58489250fec" height="80%" width="80%" />



---
<br />

<h3>Restart the Windows 10 Pro virtual machine in Azure</h3>


- Azure will flash a warning that the the Windows 10 Pro DNS servers are being updated and that you have to restart the Windows 10 Pro virtual machine.
- Here is an example of that notice.

  <img src="https://github.com/user-attachments/assets/ce24f65a-bf9e-437d-976f-18edd23f00fc" height="80%" width="80%" />


<h4>Restart the Windows 10 Pro Virtual Machine</h4>

- Navigate to the Azure Virtual Machine control panel view.
- Check the box next to your Windows 10 Pro virtual machine to select it.
- Click the `Restart` button along the top control panel as shwon below.


  <img src="https://github.com/user-attachments/assets/7c328920-f12a-401c-a2be-b118c9523e92" height="100%" width="100%" />



---
<br />

<h3>Confirm the Windows 10 Pro DNS Settings with `ipconfig /all` </h3>

- Remote into the Windows 10 Pro client machine.
- Open PowerShell.
- Run the command `ipconfig /all`.
- The DNS setting for our Windows 10 Pro virtual machine should now show that its DNS Servers is the IP address of the Windows 2022 Server.
- In this example, the Windows 10 Pro DNS Servers is now set to my Windows 2022 Server private ip address of `10.0.0.5` as shown below.


  <img src="https://github.com/user-attachments/assets/68745011-a53d-4245-9187-fdad7820a81c" height="100%" width="100%" />



---













