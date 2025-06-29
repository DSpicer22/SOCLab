<h1>Home SOC Lab using Microsoft Azure and Microsoft Sentinel</h1>


<h2>Description</h2>
This lab simulates a modern Security Operations Center (SOC) environment using Microsoft Azure, incorporating the deployment of a honeynet to attract and monitor malicious activity. It demonstrates the configuration, integration, and operational workflows of a cloud-native SIEM (Microsoft Sentinel), for detecting, analyzing, and responding to real-world cybersecurity threats.

![Image](https://github.com/user-attachments/assets/f6282110-478c-4f15-b32b-7618d660a726)

<h2>This lab covers:</h2>

- <b>Setting up a Virtual Machine in Microsoft Azure.</b>
- <b>Provisioning and configuring a Log Analytics Workspace for data collection.</b>
- <b>Routing logs to Microsoft Sentinel for centralized security monitoring.</b>
- <b>Creating a Watchlist to enrich threat detection and correlation within Microsoft Sentinel.</b>
- <b>Developing a custom workbook to visualize security insights and trends.</b>
- <b>Analyzing failed login attempts and visualizing their geographic origin to identify potential attack sources.</b>
- <b>Developing a real-time threat/attack map to monitor malicious activity across the network.</b>


<h2>Languages and Utilities Used</h2>

- <b>Kusto Query Language (KQL): Used to analyze logs and build detections/dashboards.</b>
- <b>Azure Virtual Machines: Host honeypot environments (Windows)</b> 
- <b>Azure Network Security Groups (NSGs): To control traffic and expose only certain ports (to lure attackers). </b>
- <b>Azure Storage / Log Analytics Workspace: To store logs and events from the honeypot.</b>
- <b>Data Connectors: Ingest logs from honeypots (e.g., Windows Event Logs).</b>

<h2>Program walk-through:</h2>

<p align="center">Create Resource Group</p> 

![Image](https://github.com/user-attachments/assets/7ad5e206-080e-4dde-98e7-3f0e8a8a3b34)


<p align="center">Select desired subscription, name your resource group, and select your desired Region. Once you are finished, select Review + Create to create your resource group.</p>  

![Image](https://github.com/user-attachments/assets/6ecc7164-620a-40b2-8582-f12994d0bc72)


<p align="center">Your Resource Group is created</p> 

![Image](https://github.com/user-attachments/assets/4a23c28b-6baf-43fa-ad93-b66c1801eddb)


<p align="center">Next, create a Virtual Network</p>  

![Image](https://github.com/user-attachments/assets/22bbdc16-91a7-4ede-bdd6-24fca948c00b)


<p align="center">Ensure your Virtual Network is being managed by the same Subscription and Resource Group. Once you name your Virtual Network, ensure you select the same Region location when you created your resource group. Select "Next" to continue</p> 

![Image](https://github.com/user-attachments/assets/f1afc819-2a3c-4a60-a53e-2f7acbf41e0e)


<p align="center">On the Security page, we will leave everything as default. Select "Next" to continue.</p>  

![Image](https://github.com/user-attachments/assets/e8af95e4-efdf-4597-af50-6d7f48a357a6)


<p align="center">Under the IP Addresses page, leave everything as is, and select "Next" to continue.</p>  

![Image](https://github.com/user-attachments/assets/8c90336f-2aef-4ecb-8f50-ca8718270b90)


<p align="center">Under the Tags page, you can create a tag. If you wish to not create a tag, select "Next" to continue.</p> 

![Image](https://github.com/user-attachments/assets/660eab1f-87ab-47d5-8906-535a0f0d0353)


<p align="center">Review your selections, then select "Create" to create your Virtual Network.</p>  

![Image](https://github.com/user-attachments/assets/528e79f5-8bc1-48ae-b683-3d71812784a9)


<p align="center">Under your Resource Group, you will see your virtual network has been created.</p> 

![Image](https://github.com/user-attachments/assets/4a776a57-fcde-4668-9bfe-0af63dcf0f5b)


<p align="center">Next, we will create a virtual machine.</p>  

![Image](https://github.com/user-attachments/assets/f224232b-62df-4017-bb89-dae0133191fa)


<p align="center">Ensure you select the same subscription, resource group, and region location. Name your Virtual Machine and select the availability zone of your choice.</p>

![Image](https://github.com/user-attachments/assets/8fd821e1-baab-4bf5-b624-9ce32b82ac2f)


<p align="center">Create a username and password. Ensure to select the licensing confirmation checkbox and select "Next" to continue.</p>

![Image](https://github.com/user-attachments/assets/dcafde70-efde-486a-a5a8-bf7a687bc53c)


<p align="center">Select the OS disk type of your choice and select "Next" to continue.</p>  

![Image](https://github.com/user-attachments/assets/6a5be1a4-a0b0-41ac-96dc-1fb8e0bd1abe)


<p align="center">Ensure you are using the same Virtual Network you created earlier, and to ensure you are using the default subnet.</p> 

![Image](https://github.com/user-attachments/assets/0ddaee48-473c-4457-a80a-5e4d9d8a3963)


<p align="center">Select this checkbox so that the public IP and NIC are deleted when the VM is deleted. Select "Next" to continue.</p>  

![Image](https://github.com/user-attachments/assets/d3dc31f9-12e0-43ef-9162-280b0ce1ea78)


<p align="center">Leave the settings as default. Select "Next" to continue.</p> 

![Image](https://github.com/user-attachments/assets/9557d04c-57ec-4b42-9781-6a912aaa0145)

<p align="center">Under Boot Diagnostics, select "Disable". Select "Review + Create" to conduct a validation and to review the settings on your virtual machine.</p>  

![Image](https://github.com/user-attachments/assets/fa759e91-907d-45e2-a662-49ac68534953)

<p align="center">Once validation has passed and you reviewed your settings, select "Create".</p>

![Image](https://github.com/user-attachments/assets/4d66aa08-5d31-424d-b097-b22a286f0d5e)

<p align="center">Under your resource group, you can see you virtual machine.</p> 

![Image](https://github.com/user-attachments/assets/de0ecc61-fb96-4034-b9d0-24ab55c8d1ee)

<p align="center">Next, we will be editing the Network Security Group.</p>

![Image](https://github.com/user-attachments/assets/248dfec8-b5b7-4ac0-af12-4b62cf76fee6)

<p align="center">Delete the inbound security rule for RDP.</p>

![Image](https://github.com/user-attachments/assets/75e6f364-5326-4525-ad1a-c59872cb627f)

<p align="center">Once the rule is deleted, navigate to Settings -> Inbound Security Rules</p>

![Image](https://github.com/user-attachments/assets/178db177-2730-4d6e-a78c-4994b0612577)</p>

<p align="center">Select "Add" to create an Inbound Security Rule. Only changes to make is to replace 8080 with an asterisk (*) under Destination Port Ranges. Name your inbound security rule and select "Add" to create the rule.</p>

![Image](https://github.com/user-attachments/assets/502e7a36-81e5-4718-b3df-6b1d79df8b7f)

<p align="center">Your Inbound Security Rule is created.</p>

![Image](https://github.com/user-attachments/assets/5fe0e0d6-3bb6-4850-a4fb-1768df4d39a7)

<p align="center">Navigate to your virtual machine, and locate your Public IP Address. We will start and RDP session utilizing the IP, and the username and password we created earlier.</p>

![Image](https://github.com/user-attachments/assets/f7448b7f-f5a0-4c97-80e2-0e9d91cb2f11)

<p align="center">Toggle all the settings to "No" on the Privacy Settings page and select "Accept".</p>

![Image](https://github.com/user-attachments/assets/9c337f7f-7e3a-4407-8a14-93e5da3c658a)

<p align="center">Navigate to Windows Defender Firewall Properties on your Virtual Machine</p>

![Image](https://github.com/user-attachments/assets/753748c6-34bc-4c26-98a5-1f9acd1e8d83)

<p align="center">Turn off firewall state on the Domain Profile, Private Profile, and Public Profile. Select "Apply".</p>

![Image](https://github.com/user-attachments/assets/27d70fdb-1788-47d8-af84-c1e3cb8f1b88)

<p align="center">Ping your Virtual Machines IP. The output from the Orange box is when the VM's firewall was on. The output from the Green box is when the VM's firewall was turned off, which in this case, you are able to ping your VM.</p>

![Image](https://github.com/user-attachments/assets/19d9424c-5543-4b35-9390-2b0f978fdd13)

<p align="center">Sign out of your VM, and attempt to sign back in using a different username and password. Intentional sign in failures are for the logs</p>

![Image](https://github.com/user-attachments/assets/2467e63f-3e8e-4a06-8fc4-1f91091c2838)

<p align="center">Login attempt #2 </p>

![Image](https://github.com/user-attachments/assets/b8e7c2b1-8a23-4adb-95a9-0d28cfdcf9dc)

<p align="center">Login attempt #3, using a different username and password.</p>

![Image](https://github.com/user-attachments/assets/893aa6f9-bbd7-46e3-83ec-a5afa3917ac8)

<p align="center">Log into your VM with the username and password you previously setup, navigate to Event Viewer, select "Security", select "Filter Current Log", and type in "4625" in the "All Event IDs" field. Select "Ok".</p>

![Image](https://github.com/user-attachments/assets/b7b06275-bd5f-409f-af28-021d0141e2c6)

<p align="center">Event ID 4625 shows the 3 login attempts that were made previously. Notice that this is the 1st login attempt from username "user1".</p>

![Image](https://github.com/user-attachments/assets/3bfd467e-88d4-45fe-aece-9a891ef92ed0)

<p align="center">Log entry from the 2nd attempt using the username "user1".</p>

![Image](https://github.com/user-attachments/assets/d5a1f1b5-d434-47f4-ae26-625f3b8ebf82)

<p align="center">Log entry from the 3rd attempt using the username "user2".</p>

![Image](https://github.com/user-attachments/assets/0d4b708f-f3b6-4558-bf13-08c4c7c1d4fe)

<p align="center">Navigate back to the Azure Portal, and select "Create" to create a Log Analytics Workspace, where we will then forward our event viewer logs from our Virtual Machine, into this workspace.</p>

![Image](https://github.com/user-attachments/assets/c44fe2b9-c53a-44ba-bc1b-9f7e1ddedfc0)

<p align="center">Ensure you are selecting the Azure subscription you created earlier, as well as the Resource Group. Name your Log Analytics Workspace, and ensure you are selecting the same Region you selected previously. Then select "Review + Create".</p>

![Image](https://github.com/user-attachments/assets/e6e91a55-d215-4c12-be4c-5f509da9cccb)

<p align="center">Wait a few moments until the validation is completed.</p>

![Image](https://github.com/user-attachments/assets/0ae057a9-abde-4f84-8a55-8dd435f92d94)

<p align="center">Once the validation passed, select "Create".</p>

![Image](https://github.com/user-attachments/assets/f6330386-cbb3-4807-949a-f52fd0528419)

<p align="center">You Log Analytics Workspace deployment is complete.</p>

![Image](https://github.com/user-attachments/assets/fc2679c7-715c-4592-a064-6d35e3b1d418)

<p align="center">Navigate to Microsoft Sentinel and select "Create". We are going to add our Log Analytics Workspace to Microsoft Sentinel.</p>

![Image](https://github.com/user-attachments/assets/517e5fa9-2eda-47d7-8882-94a422666ef6)

<p align="center">Select your Log Analytics Workspace you created earlier, and select "Add". If you do not see your Log Analytics Workspace, ensure that your Log Analytics Workspace deployment is complete before completing this step.</p>

![Image](https://github.com/user-attachments/assets/594aaf93-b40a-4c1f-ae95-80cfd75f6d03)

<p align="center">Microsoft Sentinel is now added to your Log Analytics Workspace. We will now configure the Azure Monitoring Agent Security Event Connector, which will create a connection between our Virtual Machine and our Log Analytics Workspace and Sentinel, in order for us to analyze the event logs in Microsoft Sentinel.</p>

![Image](https://github.com/user-attachments/assets/0b98c9f1-914e-4b1e-8bd8-03a974b8c0b5)

<p align="center">Navigate to "Content Management on the left pane, and select "Content Hub". You can also select the "Go to content hub" option as well.</p>

![Image](https://github.com/user-attachments/assets/44855545-b0c8-4922-aae4-dcaded9ddb1d)

<p align="center">Type "Windows Security Events" in the search bar. Select the checkbox for "Windows Security Events". Select "Install"</p>

![Image](https://github.com/user-attachments/assets/3617edcb-3e91-41f0-8735-b1afad684b4d)

<p align="center">Wait a few moments for the install to finish.</p>

![Image](https://github.com/user-attachments/assets/1dced37b-bddf-4128-8ecf-68df285642df)

<p align="center">Once the install is complete, select "Windows Security Events", and select "Manage".</p>

![Image](https://github.com/user-attachments/assets/452ca096-1eff-4f25-bdbe-4ce742097f21)

<p align="center">Select "Windows Security Events via AMA", and select "Open connector page".</p>

![Image](https://github.com/user-attachments/assets/b3a61b7e-4a3c-4cc7-8e05-b12345223d44)

<p align="center">Select "Create data collection rule". The data collection rule is used by the Virtual Machine to forward logs into our Log Analytics Workspace, which will allow us to access the forwarded logs inside of our SIEM.</p>

![Image](https://github.com/user-attachments/assets/e0f29bae-c1c1-4e33-955d-7dbf2c41c7e1)

<p align="center">Name your Data Collection Rule. Ensure that you are selecting the same Subscription and Resource Group you created earlier. Select "Next: Resources".</p>

![Image](https://github.com/user-attachments/assets/a9570fa5-50bd-4681-be3f-689b502080f1)

<p align="center">Click on the drop down arrows to ensure all checkboxes are selected. Then select "Next: Collect".</p>

![Image](https://github.com/user-attachments/assets/9393b31b-d1a5-4101-8b84-1fa9d850cf13)

<p align="center">Select "All Security Events". Then select "Review + Create".</p>

![Image](https://github.com/user-attachments/assets/07c5b0e8-b45e-4e61-a7c0-e6b7f8e0d2d8)

<p align="center">Once the validation has passed, select "Create".</p>

![Image](https://github.com/user-attachments/assets/04f215d5-ac5f-4840-86e9-541a6fbb57f3)

<p align="center">Data Collection Rule is created.</p>

![Image](https://github.com/user-attachments/assets/a11c6cea-90b4-4209-8d06-15aa10745e16)

<p align="center">Navigate to the Virtual Machines services in the Azure Portal and select your Virtual Machine.</p>

![Image](https://github.com/user-attachments/assets/6b11d343-6a41-4c7c-826f-aba759962d99)

<p align="center">Navigate to "Settings", and select "Extensions + Applications".</p>

![Image](https://github.com/user-attachments/assets/f53dfb88-a856-41a2-bb78-b97a3460484a)

<p align="center">Your extension is created.</p>

![Image](https://github.com/user-attachments/assets/085c7206-5589-4b3c-8f53-749aa1dfc842)

<p align="center">Navigate to the Log Analytics Workspace services in the Azure Portal and select your Log Analytics Workspace you created earlier.</p>

![Image](https://github.com/user-attachments/assets/a26cc37e-4cd6-438c-a558-55b81521cc05)

<p align="center">Navigate to "Logs", type in "SecurityEvent" as shown, and select "Run". Logs are successfully being forwarded from the Virtual Machine and into the Log Analytics Workspace. Notice in this example that an external user attempted to log into the Virtual Machine twice, with the username "Test".</p>  

![Image](https://github.com/user-attachments/assets/4a013ce4-983e-41c5-949e-dbeb86d0e5da)

<p align="center">Failed log in attempt from username "Test".</p>

![Image](https://github.com/user-attachments/assets/ab12e595-3a5f-4cea-840b-fb83c6d75ff1)

<p align="center">Retrieving IP to conduct a geo IP lookup.</p>

![Image](https://github.com/user-attachments/assets/986c8179-26d8-4f7e-8946-8c6960c1b339)

<p align="center">The IP originated from Kaunas, Lithuania.</p>

![Image](https://github.com/user-attachments/assets/5bda9c2c-f5f9-476b-8981-56f992c86613)

<p align="center">You can use the following query to output the Time Generated, Account, Computer, EventID, Activity, and IP Address.</p>

![Image](https://github.com/user-attachments/assets/013b9582-59c2-4fa4-8d6b-8e1bfd446928)

<p align="center">This query targets only Event ID 4625 (Failed log in attempt). Notice there is 1000+ failed log in attempts from VM's CORP-NET-EAST-4, AWS-Keys-Archive and NET-CORP-EAST-1.</p>

![Image](https://github.com/user-attachments/assets/5687c1cd-fc2f-48d7-9f30-a36aab9ba6b0)

<p align="center">Navigate to the Microsoft Sentinel Resource in the Azure Portal, select your Log Analytics Workspace you created earlier, navigate to "Configuration" and select "Watchlist". </p>

![Image](https://github.com/user-attachments/assets/f9ff0d6b-c8fd-4426-8154-acd3f351160f)

<p align="center">Select "New". </p>

![Image](https://github.com/user-attachments/assets/ca300ed4-2325-47a5-8d9c-3a80bd62878d)

<p align="center">Name your watchlist. For this example, the name will be "geoip" and the alias will be "geoip". Select "Next: Source".</p>

![Image](https://github.com/user-attachments/assets/a78c63b3-7978-44db-b32e-2d96a5ecf566)

<p align="center">Select your geo ip csv file for your watchlist.</p>

![Image](https://github.com/user-attachments/assets/fba80f9e-30f5-4028-ad23-ca655c454dbb)

<p align="center">Select "network" beside the "SearchKey" field. Select "Review + Create".</p>

![Image](https://github.com/user-attachments/assets/97d553d3-0910-45d3-b563-458ea9cf3120)

<p align="center">Select "Create".</p>

![Image](https://github.com/user-attachments/assets/f485db1c-93a6-4909-af49-03641633a5bd)

<p align="center">Wait a few moments for the upload to finish.</p>

![Image](https://github.com/user-attachments/assets/6115a693-22e9-4dad-ac13-15c1dcd4e1da)

<p align="center">Upload succeeded.</p>

![Image](https://github.com/user-attachments/assets/f4b097b3-0989-433d-ae30-d9ef3a0a026e)

<p align="center">The following query pulls from the geoip csv file, and outputs Time Generated, Computer, AttakcerIP, CityName, CountryName, Latitude and Longitude.</p>

![Image](https://github.com/user-attachments/assets/a25e3666-7e9f-469a-b55d-a5d659d8a07a)

<p align="center">Navigate back to the Microsoft Sentinel resource in the Azure Portal, ensure you select the Log Analytics Workspace you created earlier, navigate to "Threat MAnagement" and select "Workbooks".</p>

![Image](https://github.com/user-attachments/assets/8142a2d2-718f-47b8-9a6a-5e434852f4c7)

<p align="center">Select "Add Workbook".</p>

![Image](https://github.com/user-attachments/assets/fbdefc70-c2f0-4822-9fab-97fc36d6356b)

<p align="center">Select "Edit".</p>

![Image](https://github.com/user-attachments/assets/5ca82a37-4295-4a22-b7da-2ec1f2153abc)

<p align="center">Remove default configuration.</p>

![Image](https://github.com/user-attachments/assets/63e19d22-93b0-4443-be6d-a2fd5ce49cdb)

<p align="center">Remove the second default configuration.</p>

![Image](https://github.com/user-attachments/assets/604956ee-3a29-4b17-bd45-730638a0818f)

<p align="center">Select "Add query".</p>

![Image](https://github.com/user-attachments/assets/16b68c47-9fd6-41c8-bba2-27bf3af9cb64)

<p align="center">Select "Advanced Editor".</p>

![Image](https://github.com/user-attachments/assets/25d4dfa7-0d66-4e50-9855-56aaec01d440)

<p align="center">Remove any pre-existing text that is in the editor so we can input our JSON representation.</p>

![Image](https://github.com/user-attachments/assets/f38a6e99-8811-4102-a521-71d1c56e1675)

<p align="center">Input your query.</p>

![Image](https://github.com/user-attachments/assets/bef66ac6-7c2f-47c0-a177-98c531fc6b94)

<p align="center">Select "Done Editing".</p>

![Image](https://github.com/user-attachments/assets/c6beb2ac-8cc4-4df8-8c39-e9802008df86)

<p align="center">Select "Save".</p>

![Image](https://github.com/user-attachments/assets/8fff9430-50f2-4a22-899c-9b8ce255fb43)

<p align="center">Name your workbook and ensure you select the Resource Group you created earlier. Select "Save As".</p>

![Image](https://github.com/user-attachments/assets/1b94fa76-a719-458f-bf21-f52eb3ae30e6)

<p align="center">You can refresh at any time, or you can set up an auto refresh as well. In this example, notice that the top 3 areas with the most failed login attempts are from Argentina, Netherlands, and Poland.</p>

![Image](https://github.com/user-attachments/assets/59397365-2740-40e9-a2da-9904812eb1f9)

<p align="center">Select "Edit".</p>

![Image](https://github.com/user-attachments/assets/d298ade1-cb21-4a5d-a687-ba4523c931eb)

<p align="center">Select "Edit" on the bottom right.</p>

![Image](https://github.com/user-attachments/assets/c9c5eff1-e879-4a54-8573-4ef9b8afe0ca)

<p align="center">Select "Map Settings".</p>

![Image](https://github.com/user-attachments/assets/494c69a9-f5ee-46b3-b214-51670d274ebe)

<p align="center">Here, you can modify your map based off of your preference. Once finished, select "Apply". </p>

![Image](https://github.com/user-attachments/assets/3393ad21-6f89-48d1-ab18-632ee07bd2af)



