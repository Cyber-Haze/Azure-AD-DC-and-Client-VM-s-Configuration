
<p align="center">
<img width="300" height="168" alt="Active Directory" src="https://github.com/user-attachments/assets/ab43591a-2189-41f9-875f-b6e6c8971685" />


</p>

<h1>Preparing Active Directory Infrastructure in Azure</h1>
<p> This tutorial outlines the creation and configuration of Active Directory VMs within Azure.
</p>
<br />

<h2>High-Level Deployment and Configuration Steps(Part 1)</h2>
<p> 
  
  1. Set up resources in Azure: Create a Domain Controller and a Client VM.
  2. Configure DNS IP Address
  3. Ensure network connectivity between the Client and the Domain Controller.
  
</p>
<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Powershell

<h2>Operating Systems Used </h2>

- Windows Server 2022 (Domain Controller)
- Windows 10 (Client)

<h2> Infrastructure Overview </h2> 

<img width="829" height="708" alt="Screenshot 2025-09-03 140121" src="https://github.com/user-attachments/assets/60af5984-77a3-4a42-ac63-111dda0ce365" />
<br />

<p>

## Step 2: Create the Virtual Machines

**Create two VMs (Azure) in the same VNET. One will be a Domain Controller, the other will be a Client machine**

- Create a VM for the Domain Controller on Azure.
- Name it DC-1
- Select Windows Server 2022 Datacenter: Azure Edition -x64 Gen2 as the Image
- Size (Standard_D2_v3 - 2 vcpus, 8 GiB memory)
- Create a username and password for your VM DC-1


<img width="738" height="428" alt="Screenshot 2025-09-18 104915" src="https://github.com/user-attachments/assets/8f9134be-eaf9-4db2-80d6-7039f48289bc" />

**Create a VM for the client machine**

- Create a new VM
- Name it Client-1
- Slect Windows 10 Pro as the image
- Select at least 2 vcpus and 16 GiB memory
- Create a username and password for your VM Client-1
- Make sure to select the same resource group and Virtual network from the DC-1 VM


<img width="767" height="443" alt="image" src="https://github.com/user-attachments/assets/3fc81412-8f1b-431b-930a-2f0b97801b4f" />





<p>




## Step 3: Set the Domain Controller's Private IP to static

**We don't want the IP to change so that the client-1 machine can join the Domain**

- Once the VM has been deployed, proceed to the VM overview page and select "Networking" > network settings on the left side.

<img width="771" height="444" alt="Screenshot 2025-09-04 142451" src="https://github.com/user-attachments/assets/df88c968-13d6-4f79-a582-0f17864679da" />


- Select Network Interface Card > IP configurations > ipconfig1 and set Private IP addess allocation to static.

<img width="769" height="442" alt="Screenshot 2025-09-04 142507" src="https://github.com/user-attachments/assets/56ae10dd-ee06-402f-b428-52d20ff8dd85" />







<p>



## Step 4: Login to DC-1 and configure the firewall settings

**We're going to configure the firewall settings inside our Domain Controller virtual machine**

- Right click the start menu and click Run
- Type in wf.msc
- This will open Windows Defender Firewall with Advanced Security

<img width="411" height="232" alt="Screenshot 2025-09-04 143238" src="https://github.com/user-attachments/assets/93eb7161-6a6f-45a2-a81f-af9aaec633e5" />



- Disable the Firewall settings by going to "Windows Defender Firewall Properties"
- Turn the Domain Profile firewall off
- Turn the Private Profile firewall off

<img width="396" height="449" alt="Screenshot 2025-09-04 143329" src="https://github.com/user-attachments/assets/9794eec5-424d-456a-abe3-37758580b2ad" />

- You should be left with these settings

<img width="405" height="250" alt="Screenshot 2025-09-04 143310" src="https://github.com/user-attachments/assets/493b8efa-883c-45c7-91dd-09f1038e4cda" />











<p>


## Step 5: Login to Client-1 and ping the Domain Controller

**We're going to use Powershell to ping the Domain Controller VM**

- Login to Client-1
- Look up Powershell in the search bar
- Ping the Domain controller's private IP address (10.0.0.4)

<img width="675" height="398" alt="Screenshot 2025-09-18 111944" src="https://github.com/user-attachments/assets/c0601cac-60f3-43e6-be2b-af1f5b9fb023" />





<p>


## Step 6: In Powershell, type ipconfig /all 

**The output for the DNS settings should show DC-1's private IP address**

- In Powershell, type in ipconfig /all
- You should see the DNS servers IP should be set to 10.0.0.4 (DC-1's private IP address)

<img width="1996" height="1249" alt="Screenshot 2025-09-03 203420" src="https://github.com/user-attachments/assets/9578cc37-2f34-4305-83b6-2ed99166245b" />


- That's it for this portion, see you in the next portion of the lab :)


<p>
