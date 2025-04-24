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

<img src="https://github.com/user-attachments/assets/acef0270-3381-4c67-b464-c06206cc42e1" height="80%" width="80%" />



---
<br />

<h3>Confirm your Windows Server is using a Static IP Address</h3>

- Azure will update the Windows Server network interface and then confirm the change has been made as shown below.

<img src="https://github.com/user-attachments/assets/c708bb36-9999-4622-967e-e37948911ff9" height="40%" width="40%" />

- Now your Windows Server will show its private IP address as being static as shown below.


<img src="https://github.com/user-attachments/assets/d9196407-4fef-4743-93d8-b227d82f68be" height="80%" width="80%" />


---
<br />




