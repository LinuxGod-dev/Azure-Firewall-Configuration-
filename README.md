# Azure-Firewall-Configuration-
Installation of Azure Firewall

# Objective

- Deploy and test an Azure Firewall

# Skills Learned

- Cloud Security & Firewall	
 - Azure Firewall Deployment & Configuration: Implementing Azure Firewall as a centralized network security service.
 - Network Rule Configuration (L3/L4): Creating rules to control outbound network traffic, such as allowing external DNS server lookups (UDP port 53)
 - Application Rule Configuration (L7): Implementing Layer 7 filtering by creating application rules to specifically allow outbound traffic to a target FQDN (e.g., www.bing.com).

- Infrastructure as Code (IaC)
 - Azure Resource Manager (ARM) Templates: Utilizing and deploying infrastructure from an ARM template (template.json) to provision virtual networks and virtual machines.

- Cloud Networking & Routing
 - User-Defined Routes (UDR): Creating a custom or "default route" to enforce traffic inspection by directing all outbound workload traffic through the firewall.
 - Virtual Network (VNet) Management: Establishing a secure virtual network environment with multiple subnets (Workload-SN, Jump-SN)
 - Custom DNS Configuration: Configuring the virtual machine's network interface to use specific custom DNS servers for name resolution.

- Validation & Operations
 - Security Testing/Validation: Performing connectivity and policy enforcement tests to validate that traffic is correctly allowed (e.g., bing.com) and denied (e.g., microsoft.com) by the firewall.
 - PowerShell Scripting: Using basic PowerShell commands (e.g., Remove-AzResourceGroup) for resource cleanup and management.
 - Remote Administration: Connecting to Azure Virtual Machines via Remote Desktop Protocol (RDP)

# Tools Used

- Azure Firewall: The core security service being deployed and configured to provide network (L3/L4) and application (L7) traffic filtering.
- Azure Portal The web-based graphical interface used for:
  - Deploying the ARM template.
  - Configuring the firewall policies, rules, and network settings.
  - Managing and inspecting all deployed resources (VMs, VNets, etc.).
-Azure Resource Manager (ARM) Template	Used as the Infrastructure as Code (IaC) tool (template.json) to provision the foundational network infrastructure (Virtual Networks, Subnets, Virtual Machines).
- Azure Cloud Shell (PowerShell)	Used for managing and cleaning up Azure resources, specifically running the Remove-AzResourceGroup command.
- Azure Virtual Machines (VMs)	The Jump-VM and Workload-VM serve as the test endpoints to validate the connectivity and policy enforcement of the firewall.
- Remote Desktop Protocol (RDP)	The protocol used to remotely connect and interact with the Azure Virtual Machines to perform connectivity tests.
- User-Defined Routes (UDR)	A networking configuration tool/feature used to direct all outbound traffic from the workload subnet to the Azure Firewall for inspection.

# Steps
Exercise 1: Deploy and test an Azure Firewall 
Estimated timing: 40 minutes 
For all the resources in this lab, we are using the East (US) region. Verify with your instructor this is the 
region to use for your class. 
In this exercise, you will complete the following tasks: 
• Task 1: Use a template to deploy the lab environment. 
• Task 2: Deploy an Azure firewall. 
• Task 3: Create a default route. 
• Task 4: Configure an application rule. 
• Task 5: Configure a network rule. 
• Task 6: Configure DNS servers. 
• Task 7: Test the firewall. 
Task 1: Use a template to deploy the lab environment. 
In this task, you will review and deploy the lab environment. 
In this task, you will create a virtual machine by using an ARM template. This virtual machine will be used 
in the last exercise for this lab. 

1. Sign-in to the Azure portal https://portal.azure.com/. 
Sign in to the Azure portal using an account that has the Owner or Contributor role in the Azure 
subscription you are using for this lab. In this Cloudslice lab, this account is LabUser
56666614@LODSPRODMCA.onmicrosoft.com with password kpu^*4y=.

<img width="1366" height="768" alt="Screenshot (723)" src="https://github.com/user-attachments/assets/8b5415d7-0fd4-4595-8169-3f5fc28d2297" />

2. In the Azure portal, in the Search resources, services, and docs text box at the top of the 
Azure portal page, type Deploy a custom template and press the Enter key.

<img width="1366" height="768" alt="Screenshot (725)" src="https://github.com/user-attachments/assets/cea21198-5df6-4e3f-88a3-800a547706e1" />

3. On the Custom deployment blade, click the Build your own template in the editor option.

<img width="1366" height="768" alt="Screenshot (726)" src="https://github.com/user-attachments/assets/7cbe0f07-2548-4aa4-8cd2-15c86c0fbe16" />

4. On the Edit template blade, click Load file, locate the \Allfiles\Labs\08\template.json file 
and click Open. 
Review the content of the template and note that it deploys an Azure VM hosting Windows Server 
2016 Datacenter.

<img width="1366" height="768" alt="Screenshot (727)" src="https://github.com/user-attachments/assets/91ee6db8-ad17-4fa9-885f-3ea26ea89cce" />

5. On the Edit template blade, click Save.

<img width="1366" height="768" alt="Screenshot (728)" src="https://github.com/user-attachments/assets/48c11f5f-ac4f-4082-9ea7-7c717faf56e5" />

6. On the Custom deployment blade, ensure that the following settings are configured (leave 
any others with their default values):

<img width="1366" height="768" alt="Screenshot (730)" src="https://github.com/user-attachments/assets/a310b2c2-0f64-4a4c-bc69-fd165abdabf3" />

7. Click Review + create, and then click Create. 
Wait for the deployment to complete. This should take about 2 minutes.

<img width="1366" height="768" alt="Screenshot (731)" src="https://github.com/user-attachments/assets/ddfa6d2e-04bb-411e-8fad-71c5872fcb9b" />

<img width="1366" height="768" alt="Screenshot (732)" src="https://github.com/user-attachments/assets/612c3967-8930-4fab-84ce-952f98f64b79" />

Task 2: Deploy the Azure firewall 
In this task you will deploy the Azure firewall into the virtual network. 
1. In the Azure portal, in the Search resources, services, and docs text box at the top of the 
Azure portal page, type Firewalls and press the Enter key.

<img width="1366" height="768" alt="Screenshot (733)" src="https://github.com/user-attachments/assets/6e17dbfd-41cb-44aa-af83-51ccecf76403" />

2. On the Firewalls blade, click + Create.

<img width="1366" height="768" alt="Screenshot (734)" src="https://github.com/user-attachments/assets/ab562458-c99b-4da1-9692-c9d696303834" />

3. On the Basics tab of the Create a firewall blade, specify the following settings:

<img width="1366" height="768" alt="Screenshot (735)" src="https://github.com/user-attachments/assets/1305fa0b-3273-4aa4-905e-1506ceccb315" />

<img width="1366" height="768" alt="Screenshot (737)" src="https://github.com/user-attachments/assets/d430b475-e4ee-40df-ba00-25af55ce9d55" />

4. Click Review + create and then click Create. 
Wait for the deployment to complete. This should take about 5 minutes. 

<img width="1366" height="768" alt="Screenshot (738)" src="https://github.com/user-attachments/assets/ecfce9d2-70da-421a-8b01-736205abb7b0" />

5. In the Azure portal, in the Search resources, services, and docs text box at the top of the 
Azure portal page, type Resource groups and press the Enter key.

<img width="1366" height="768" alt="Screenshot (739)" src="https://github.com/user-attachments/assets/d4354a00-f919-493b-8ada-a9b61f811388" />

6. On the Resource groups blade, in the list of resource group, click the AZ500LAB08 entry. 
On the AZ500LAB08 resource group blade, review the list of resources. You can sort by Type.

<img width="1366" height="768" alt="Screenshot (740)" src="https://github.com/user-attachments/assets/5d1907a3-7196-41f9-b289-59c65517af26" />

7. In the list of resources, click the entry representing the Test-FW01 firewall.

<img width="1366" height="768" alt="Screenshot (742)" src="https://github.com/user-attachments/assets/1d7ded4a-0a72-46d7-b93b-c1a215371f51" />

8. On the Test-FW01 blade, identify the Private IP address that was assigned to the firewall. 
You will need this information in the next task. 

<img width="1366" height="768" alt="Screenshot (743)" src="https://github.com/user-attachments/assets/3ed00a4c-34a6-47e6-b701-158290e2559e" />

Task 3: Create a default route 
In this task, you will create a default route for the Workload-SN subnet. This route will configure outbound 
traffic through the firewall. 
1. In the Azure portal, in the Search resources, services, and docs text box at the top of the 
Azure portal page, type Route tables and press the Enter key.

<img width="1366" height="768" alt="Screenshot (744)" src="https://github.com/user-attachments/assets/54a09da0-6f98-48fd-b665-5962feb52194" />

2. On the Route tables blade, click + Create.

<img width="1366" height="768" alt="Screenshot (745)" src="https://github.com/user-attachments/assets/8a28ab32-8720-4ff3-9d7a-809d64e1a686" />

3. On the Create route table blade, specify the following settings:

<img width="1366" height="768" alt="Screenshot (746)" src="https://github.com/user-attachments/assets/59ddf1e7-4f86-45d6-9369-17187fd40c98" />

4. Click Review + create, then click Create, and wait for the provisioning to complete.

<img width="1366" height="768" alt="Screenshot (747)" src="https://github.com/user-attachments/assets/9c086a72-4ec6-4499-b351-a2e6226ef7c7" />

5. On the Route tables blade, click Refresh, and, in the list of route tables, click the Firewall
route entry.
<img width="1366" height="768" alt="Screenshot (748)" src="https://github.com/user-attachments/assets/8e622912-8e2d-4442-878e-eb7941652dde" />

6. On the Firewall-route blade, in the Settings section, click Subnets and then, on 
the Firewall-route | Subnets blade, click + Associate.

<img width="1366" height="768" alt="Screenshot (749)" src="https://github.com/user-attachments/assets/74698be6-d60b-4b43-b67c-3c9ea016670d" />

7. On the Associate subnet blade, specify the following settings:

<img width="1366" height="768" alt="Screenshot (750)" src="https://github.com/user-attachments/assets/56cb753d-923e-4f6a-b2d5-ff157439633d" />

8. Ensure the Workload-SN subnet is selected for this route, otherwise the firewall won't work 
correctly.

<img width="1366" height="768" alt="Screenshot (750)" src="https://github.com/user-attachments/assets/73fa9f8d-828f-4c0e-a7be-a957e2e65ef0" />

9. Click OK to associate the firewall to the virtual network subnet.

<img width="1366" height="768" alt="Screenshot (751)" src="https://github.com/user-attachments/assets/59fe7aa9-c3bd-4cf5-9078-35aa035bdf24" />

10. Back on the Firewall-route blade, in the Settings section, click Routes and then click + 
Add. 

<img width="1366" height="768" alt="Screenshot (752)" src="https://github.com/user-attachments/assets/fa2c5979-8531-43bf-8236-53508970b85e" />

11. On the Add route blade, specify the following settings:

<img width="1366" height="768" alt="Screenshot (753)" src="https://github.com/user-attachments/assets/988edaca-10a8-46e4-a263-ab95bef532ff" />

12. Click Add to add the route. 

<img width="1366" height="768" alt="Screenshot (754)" src="https://github.com/user-attachments/assets/de8a23e6-2d74-4d50-b76a-ec5417ea2e75" />

Task 4: Configure an application rule 
In this task you will create an application rule that allows outbound access to www.bing.com. 
1. In the Azure portal, navigate back to the Test-FW01 firewall.

<img width="1366" height="768" alt="Screenshot (755)" src="https://github.com/user-attachments/assets/6fa23f09-4885-48c3-ac08-2cd5eb2c0fb1" />

2. On the Test-FW01 blade, in the Settings section, click Rules (classic).

<img width="1366" height="768" alt="Screenshot (756)" src="https://github.com/user-attachments/assets/19b8bd84-6990-4607-a312-ed72668d34f4" />

3. On the Test-FW01 | Rules (classic) blade, click the Application rule collection tab, and 
then click + Add application rule collection. 

<img width="1366" height="768" alt="Screenshot (757)" src="https://github.com/user-attachments/assets/c7a95a83-8130-47c7-9cbe-b3e561a745c0" />

4. On the Add application rule collection blade, specify the following settings (leave others 
with their default values):

<img width="1366" height="768" alt="Screenshot (758)" src="https://github.com/user-attachments/assets/bc6884d4-f917-4b3d-81cb-f45fd34b6c1c" />

5. On the Add application rule collection blade, create a new entry in the Target 
FQDNs section with the following settings (leave others with their default values):

<img width="1366" height="768" alt="Screenshot (759)" src="https://github.com/user-attachments/assets/c327d187-607c-4aa1-8793-c7de489a756c" />

6. Click Add to add the Target FQDNs-based application rule. 
Azure Firewall includes a built-in rule collection for infrastructure FQDNs that are allowed by 
default. These FQDNs are specific for the platform and can't be used for other purposes.

<img width="1366" height="768" alt="Screenshot (760)" src="https://github.com/user-attachments/assets/635c30d6-1a66-41dc-a15f-2b053fd6e918" />

Task 5: Configure a network rule 
In this task, you will create a network rule that allows outbound access to two IP addresses on port 53 
(DNS). 

<img width="1366" height="768" alt="Screenshot (764)" src="https://github.com/user-attachments/assets/16023e66-916d-4181-b7af-4c3df680f54b" />


2. On the Test-FW01 | Rules (classic) blade, click the Network rule collection tab and then 
click + Add network rule collection. 

<img width="1366" height="768" alt="Screenshot (765)" src="https://github.com/user-attachments/assets/ddd502eb-968e-4efb-aa02-fe1872ada1ce" />

3. On the Add network rule collection blade, specify the following settings (leave others with 
their default values):

<img width="1366" height="768" alt="Screenshot (766)" src="https://github.com/user-attachments/assets/84807f93-3ea8-4c69-bb36-e0f9401b2197" />

4. On the Add network rule collection blade, create a new entry in the IP Addresses section 
with the following settings (leave others with their default values):

<img width="1366" height="768" alt="Screenshot (767)" src="https://github.com/user-attachments/assets/1a946329-8c95-49d1-8ab5-f05a01f334d9" />

5. Click Add to add the network rule. 
The destination addresses used in this case are known public DNS servers.

<img width="1366" height="768" alt="Screenshot (768)" src="https://github.com/user-attachments/assets/306454ce-1414-4ed5-b91f-b1156dcb288e" />

Task 6: Configure the virtual machine DNS servers 
In this task, you will configure the primary and secondary DNS addresses for the virtual machine. This is 
not a firewall requirement. 
1. In the Azure portal, navigate back to the AZ500LAB08 resource group.

<img width="1366" height="768" alt="Screenshot (769)" src="https://github.com/user-attachments/assets/48202deb-bbe5-43cb-ab65-a27ad69766ca" />

2. On the AZ500LAB08 blade, in the list of resources, click the Srv-Work virtual machine.

<img width="1366" height="768" alt="Screenshot (770)" src="https://github.com/user-attachments/assets/7aeb22ed-1277-4081-83b0-5d3f6a359a83" />

3. On the Srv-Work blade, click Networking.

<img width="1366" height="768" alt="Screenshot (771)" src="https://github.com/user-attachments/assets/559c4fc5-05db-4508-96a6-e35b0c78727b" />

4. On the Srv-Work | Networking Settings blade, click the link next to the Network 
interface entry.

<img width="1366" height="768" alt="Screenshot (772)" src="https://github.com/user-attachments/assets/06f82eac-9ff8-4051-b3c6-d75e6081e683" />

5. On the network interface blade, in the Settings section, click DNS servers, select 
the Custom option, add the two DNS servers referenced in the network 
rule: 209.244.0.3 and 209.244.0.4, and click Save to save the change. 

<img width="1366" height="768" alt="Screenshot (773)" src="https://github.com/user-attachments/assets/1073cbb3-e12e-44b1-9f30-462bc6d4a19a" />

6. Return to the Srv-Work virtual machine page.  
Wait for the update to complete. 
Updating the DNS servers for a network interface will automatically restart the virtual machine to 
which that interface is attached, and if applicable, any other virtual machines in the same 
availability set.

<img width="1366" height="768" alt="Screenshot (774)" src="https://github.com/user-attachments/assets/3db9fa62-f9d0-4e28-9865-a63fd506952b" />

Task 7: Test the firewall 
In this task, you will test the firewall to confirm that it works as expected. 
1. In the Azure portal, navigate back to the AZ500LAB08 resource group. 

<img width="1366" height="768" alt="Screenshot (775)" src="https://github.com/user-attachments/assets/a635a268-f8bf-4e9c-96ef-01dfd39499d5" />

2. On the AZ500LAB08 blade, in the list of resources, click the Srv-Jump virtual machine.

<img width="1366" height="768" alt="Screenshot (776)" src="https://github.com/user-attachments/assets/0ce24c87-7dd0-4207-95d0-ecc01ae822e3" />

3. On the Srv-Jump blade, click Connect and, in the drop down menu, click Connect.

<img width="1366" height="768" alt="Screenshot (778)" src="https://github.com/user-attachments/assets/e222937c-cd65-4c2e-bd9e-46ef3786960f" />

4. Download the RDP file and use it to connect to the Srv-Jump Azure VM via Remote 
Desktop. When prompted to authenticate, provide the following credentials:

<img width="1366" height="768" alt="Screenshot (779)" src="https://github.com/user-attachments/assets/03a09618-d881-4b9f-bc0e-9b9b7373b3eb" />

5. The following steps are performed in the Remote Desktop session to the Srv-Jump Azure VM.

6. You will connect to the Srv-Work virtual machine. This is being done so we can test the ability 
to access the bing.com website.

7. Within the Remote Desktop session to Srv-Jump, right-click Start, in the right-click menu, 
click Run, and, from the Run dialog box, run the following to connect to Srv-Work.

<img width="1366" height="768" alt="Screenshot (785)" src="https://github.com/user-attachments/assets/a43f5871-348b-47d2-9d0b-6c7df992130d" />

8. When prompted to authenticate, provide the following credentials: 

 <img width="1366" height="768" alt="Screenshot (785)" src="https://github.com/user-attachments/assets/e63769c5-0eb0-47aa-99c5-594f598281d3" />

9. Wait for the Remote Desktop session to be established and the Server Manager interface to load.

<img width="1366" height="768" alt="Screenshot (780)" src="https://github.com/user-attachments/assets/643a0289-07f7-44b8-881d-458ef8751276" />

10. Within the Remote Desktop session to Srv-Work, in Server Manager, click Local 
Server and then click IE Enhanced Security Configuration.

<img width="1366" height="768" alt="Screenshot (781)" src="https://github.com/user-attachments/assets/0a550dbc-5724-43cd-9db8-6d8f5e4b830f" />

11. In the Internet Explorer Enhanced Security Configuration dialog box, set both options 
to Off and click OK.

<img width="1366" height="768" alt="Screenshot (782)" src="https://github.com/user-attachments/assets/4307e753-6e4f-4a52-ac44-5625501a3bd6" />

12. Within the Remote Desktop session to Srv-Work, start Internet Explorer and browse 
to https://www.bing.com. 
The website should successfully display. The firewall allows you access.

<img width="1366" height="768" alt="Screenshot (783)" src="https://github.com/user-attachments/assets/adbbce02-858d-470a-933c-8dc12295e584" />

13. Browse to http://www.microsoft.com/ 
Within the browser page, you should receive a message with text resembling the following: HTTP 
request from 10.0.2.4:xxxxx to microsoft.com:80. Action: Deny. No rule matched. Proceeding with default action. This is expected, since the firewall blocks access to this 
website.

<img width="1366" height="768" alt="Screenshot (786)" src="https://github.com/user-attachments/assets/16f8a334-43c6-4cca-9134-d7ca4f67922e" />

14. Terminate both Remote Desktop sessions. 
Result: You have successfully configured and tested the Azure Firewall.

# Clean up resources 
Remember to remove any newly created Azure resources that you no longer use. Removing unused 
resources ensures you will not incur unexpected costs.

1. In the Azure portal, open the Cloud Shell by clicking the first icon in the top right of the Azure 
Portal. If prompted, click PowerShell and Create storage.

2. Ensure PowerShell is selected in the drop-down menu in the upper-left corner of the Cloud 
Shell pane.

<img width="1366" height="768" alt="Screenshot (788)" src="https://github.com/user-attachments/assets/110d720b-7d8c-415e-a071-181f43ba5a82" />


4. In the PowerShell session within the Cloud Shell pane, run the following to remove the 
resource group you created in this lab: powershellTypeCopy 
Remove-AzResourceGroup -Name "AZ500LAB08" -Force -AsJob

<img width="1366" height="768" alt="Screenshot (790)" src="https://github.com/user-attachments/assets/25e9e719-d801-4736-8d4c-2ce0ab386c45" />

4. Close the Cloud Shell pane.











 




























































