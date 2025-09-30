
<p align="center">
<img width="300" height="168" alt="Active Directory" src="https://github.com/user-attachments/assets/ab43591a-2189-41f9-875f-b6e6c8971685" />


</p>

<h1>Preparing Active Directory Infrastructure in Azure</h1>
<p> This tutorial outlines the creation and configuration of Active Directory Virtual Machines within Azure.
</p>
<br />

<h2>High-Level Deployment and Configuration Steps(Part 1)</h2>
<p> 
  
  1. Setup resources in Azure: Create a Domain Controller and a Client VM.
  2. Configure DNS IP Address: Join Client to Domain Controller 
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

<h2> Step 1: Setup resources in Azure </h2> 


<p> 
  
  - Create a Resource group (RG)
  - Create a Vitural Network (Vnet) and Subnet. link it to the Resource Group 

</p>

<p> 
  
<img width="751" height="362" alt="image" src="https://github.com/user-attachments/assets/f5f45207-20fb-4e24-bfc4-de18c37b75a3" />
<img width="755" height="636" alt="image" src="https://github.com/user-attachments/assets/e9964ca9-10da-4cfb-8350-7a2176c90e05" />

</p>

<h2></h2>
<br /> 

<p> 
  
  - Create a VM for the Domain Controller. Name: DC-1
  - Link it to the Vnet & Resource Group.
  - Ensure to select the correct OS: Windows Server 2022 Datacenter
  - Select CPU and Memory: 2CPUs minimum
  - Set Username and Password

</p>
<br />
<p> 
<img width="757" height="357" alt="Screenshot 2025-09-26 002511" src="https://github.com/user-attachments/assets/ff97bb45-019f-4a95-b8d5-5cfe3fc4c4c0" />

<img width="766" height="147" alt="image" src="https://github.com/user-attachments/assets/d4bd7a1a-cb7f-4d04-bef5-a003dd644d7b" />

</p>

<p> 
  
  In the Networking section of creating the VM, Ensure to link the Vnet created prior.
  
  Next, "Review and Create".
  
</p>

<p> 

<img width="771" height="342" alt="Screenshot 2025-09-26 002920" src="https://github.com/user-attachments/assets/00432f4e-6ed4-4885-a883-18c2bcd6f366" />

<h2></h2>

<img width="645" height="392" alt="image" src="https://github.com/user-attachments/assets/e7bf4568-7450-4b3a-9a65-ea1c43f3115d" />

</p>
<p> 

- Create a VM for the Client. Name: Client-1
- Link it to the Vnet & Resource Group, Same as DC-1
- Ensure to select the correct OS: Windows 10 Pro
- Select CPU and Memory: 2CPUs minimum
- Set Username and Password
- Review & Create

</p>

<h2></h2>

<p> 
  
<img width="1874" height="422" alt="image" src="https://github.com/user-attachments/assets/ba27f551-32dc-4d3f-8d91-35f4e84298e5" />

When both VM's have been created your portal should look like this.

</p>

<p>
<h2></h2>

<h2> Step 2:Configure DNS IP Address: Join Client to Domain Controller </h2> 

<P> 

Both VM's have been given a Private & Public IP address, however we want the client to have access to the Domain Controller at all times. That means we need to ensure the Private IP Address of DC-1 cannot change regardless of how many new Vm's are created or how many times it is restarted.

- Select DC-1
- Networking > Network Settings
- Select the Virtual NIC ( Network Interfacer Card)
- Select IP Configurations
- Select "ipconfig1" > Change allocation from Dynamic to Static & save.
- ( You can Also configure the IP address you want the Domain Controller to have.)

</P>
<p> 
  
<img width="961" height="413" alt="image" src="https://github.com/user-attachments/assets/a63d3471-84fd-4bb7-93a1-33cbca772a54" />
<img width="921" height="511" alt="image" src="https://github.com/user-attachments/assets/a0a394ec-1a55-4903-a40c-fcc7726c12df" />
<img width="460" height="393" alt="image" src="https://github.com/user-attachments/assets/d5b21d1c-bcbc-45d5-b8f9-92d8e584ec15" />

</p>

<h2></h2>

<p>
  
Next

- Select the Client-1 VM
- Networking > Network Settings
- Select the Virtual NIC ( Network Interfacer Card)
- Select  "DNS Servers"
- Change "DNS servers" from "Inherit.." to "custom"
- Enter the Private IP Address of the Domain Controller ( In this case 10.0.0.4) > Save

Both DC-1 and Client-1 are now connected 
Lets perform a test to confirm.

</p>

<p>

<img width="950" height="431" alt="image" src="https://github.com/user-attachments/assets/53b9f474-529e-44b0-bda2-ed9e6d9066d5" />
<img width="1074" height="334" alt="image" src="https://github.com/user-attachments/assets/ab111d01-2f0f-40d0-a598-c980e9900d6f" />

</p>

<h2></h2>

<h2> Step 3: Ensure network connectivity between the Client and the Domain Controller. </h2>

<p> 
  
In order to confirm connectivity between Domain Controller and Client, we need to allow ICMP traffic through the Firewall of DC-1.

After which we can run a "ping" command from the Client VM to DC-1.

Here are the steps 

  - Log into DC-1 with created Username and Password
  - Press Win+R to launch Run Protocol
  - type wf.msc to open
  - This will open Windows Defender Firewall with Advanced Security
  - Select Inbound rules
  - Highlight all Inbound "ICMP" rules and enable them 

</p>

<br/>

<p> 

<img width="411" height="227" alt="image" src="https://github.com/user-attachments/assets/446bbd6a-4236-476f-a345-c4bb9c5a0c18" />
<img width="1257" height="441" alt="image" src="https://github.com/user-attachments/assets/c64fd07a-be33-4723-b9e1-390466baf5b1" />
<img width="1117" height="388" alt="image" src="https://github.com/user-attachments/assets/baffd6a8-fa7b-4866-b491-1a48d57eac89" />

</p>

<h2></h2>

<p> 

You can also Disable the firewall entirely

- Disable the Firewall settings by going to "Windows Defender Firewall Properties"
- Turn the Domain Profile firewall off
- Turn the Private Profile firewall off

</p>

<p> 
<img width="396" height="449" alt="Screenshot 2025-09-04 143329" src="https://github.com/user-attachments/assets/9794eec5-424d-456a-abe3-37758580b2ad" />

- You should be left with these settings

<img width="405" height="250" alt="Screenshot 2025-09-04 143310" src="https://github.com/user-attachments/assets/493b8efa-883c-45c7-91dd-09f1038e4cda" />

</p>

<h2></h2>

<h2> Login to Client-1 and ping the Domain Controller </h2>

<p> </p>

**We're going to use Powershell to ping the Domain Controller VM**

- Login to Client-1
- Look up Powershell in the search bar, run as admin.
- Ping the Domain controller's private IP address (10.0.0.4)
    - command: "ping 10.0.0.4" 
- Once "ping" is successfull return to DC-1 and diable ICMP traffic in the firewall. ( or enable the firewall if you had disabled it entirely) 

</p>

<p> 
  
<img width="693" height="677" alt="image" src="https://github.com/user-attachments/assets/97ca3b2f-25e1-4440-b4bb-0983e3be2aae" />
<img width="486" height="300" alt="image" src="https://github.com/user-attachments/assets/26ac374b-236f-4d37-ae0f-abb219e97775" />

</p>

<h2></h2>

<p> 

The output for the DNS settings also should show DC-1's private IP address

 - In Powershell, type in ipconfig /all
 - You should see the DNS servers IP should be set to 10.0.0.4 (DC-1's private IP address)

<img width="582" height="474" alt="image" src="https://github.com/user-attachments/assets/642d6168-79f3-425d-ac21-be70dc028e51" />


</p>

<h3> Now we can move to Phase 2: Configuring Active Directory </h3>

