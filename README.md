![License: CISCO](https://img.shields.io/badge/License-CISCO-blue.svg) [![published](https://static.production.devnetcloud.com/codeexchange/assets/images/devnet-published.svg)](https://developer.cisco.com/codeexchange/github/repo/aligarci/swc_amp_securex_orchestration)

# swe_anyconnect_amp_securex_orchestration
Workflow of SecureX Action Orchestrator module.
Disclaimer: This code requires to be implemented in SecureX Orcehstrator.
 
# Author:
- Hanna Jabbour
 

# Motivation
The goal of this workflow is to trigger a response based on a Stealtwhatch Alarm/Event. The alarm is shared from Stealhwatch to Cisco SecureX Incident Manager through the SWE SecureX integration implemented with Stealtwhatch version 7.2.1. The incident details are read and parsed by the SecureX Orchestrator, the information extracted includes the source IP of the alarm and the time stamps. This information is then used to extract flows and process hashes from Stealthwatch, the process hashes and IPs are used to provide a response using AMP, The process hash is blocked across the enterprise and the host is isolated.  


![alt text](https://github.com/hanjabbo/SWE_Anyconnect_AMP_SecureX_Orchestration/blob/master/Orchestration_.png) 



# Scenario
- Remote or local worker connected to the network.
- The end device has AnyConnect and AMP for endpoint installed for endpoint security and connectivity
- AnyConnect is integrated with SWE to share process information and flow information
- Stealthwatch is monitoring the network end to end
- Stealthwatch is integrated with Cisco Threat Resposne, SecureX Orcehstrator.


![alt text](https://github.com/hanjabbo/SWE_Anyconnect_AMP_SecureX_Orchestration/blob/master/scenario.png) 



# Workflow steps


![alt text](https://github.com/hanjabbo/SWE_Anyconnect_AMP_SecureX_Orchestration/blob/master/Steps_.png) 


1. Device initiate a suspicious behavior (The device monitored by Stealthwatch, has AMP and Anyconnect installed)
2. SWE triggers an alarm and send it to Threat Resposne as an incident
3. SecureX Orcehstrator parse the incident details, indulding IP and time frame
5. SecureX Orcehstrator query Stealthwatch for the flows and process hashes involved in the communication occuring at that time frame from that specific IP.
6. Find AMP Identified (GUID) assosiated with the IP that is the source of the malicious behavior
7. Block the Process Hashes extracted from SWE, using AMP
7. Isolate host with AMP 
8. Send a message to a webex teams room notifying about the endpoint isolation and the proceess hashes blocking


![alt text](https://github.com/hanjabbo/SWE_Anyconnect_AMP_SecureX_Orchestration/blob/master/Webex_Teams_.png) 


![alt text](https://github.com/hanjabbo/SWE_Anyconnect_AMP_SecureX_Orchestration/blob/master/Workflow_.png) 




# How to use it

## Action Orchestration configuration
- Log into Action Orchestration 
- Click on workflow tab
- Click on import
- Import from: "browse"
- Paste JSON file content into text box
- Check "import as a new workflow (clone)
- Click on import
- Now the workflow is imported. You can click on it and will be able to modify it:


![alt text](https://github.com/hanjabbo/SWE_Anyconnect_AMP_SecureX_Orchestration/blob/master/Workflow2_.png) 

- CLick on "HJ_SWE_Incident" and choose the incident to process this incident can be fed to the workflow manualy or through an automate incident collection
![alt text](https://github.com/hanjabbo/SWE_Anyconnect_AMP_SecureX_Orchestration/blob/master/Incident_.png)
- On the right pannel you can paste the link of the incident or pass it as a variable

- Click on "SWE_Get_DomainID", on the right pannel you will be able to add or modify the target and account keys for your SWE installation
![alt text](https://github.com/hanjabbo/SWE_Anyconnect_AMP_SecureX_Orchestration/blob/master/Domain_.png)
- Apply the same Target and account keys on  the "SWE_Flows_Process_Hashes"

- Click on "HJ_Add_Hash_To_Block_Lists" On the right pannel, you will be able to add the target and account keys of your AMP4E endpoints.
![alt text](https://github.com/hanjabbo/SWE_Anyconnect_AMP_SecureX_Orchestration/blob/master/HashBlock_.png) 
- Apply the same Target and account keys on  the "HJ_AMP_Get_Guid for IP" and "HJ_AMP-isolate host"


- Click on "Webex Teams Post a Message" and include:

![alt text](https://github.com/hanjabbo/SWE_Anyconnect_AMP_SecureX_Orchestration/blob/master/WebeXTeams2_.png) 


   - The access token of your Webex Teams bot (You can create one and obtain the acces token here: https://developer.webex.com/)
   - Also add the room id of your webex room, where you want to post the message (You can obtain your room id going to https://developer.webex.com/docs/api/v1/rooms/get-room-details and pasting the name of your room)
   

![alt text](https://github.com/hanjabbo/SWE_Anyconnect_AMP_SecureX_Orchestration/blob/master/Webex_Room_ID.png)


