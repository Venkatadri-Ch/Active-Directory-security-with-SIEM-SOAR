# Active-Directory-security-with-SIEM-SOAR
A security project focused on monitoring and protecting Active Directory using Splunk (SIEM) and Shuffle (SOAR). It detects unauthorized logins, triggers automated alerts, disables compromised domain users via Active Directory app in Shuffle, and sends notifications through Telegram and Email. and Email.

### Objective

The goal of this project is to simulate a real-world attack on an Active Directory environment and set up automated detection and response using Splunk and Shuffle SOAR. An Active Directory was used to manage domain users, while Splunk collected and monitored security events. When an unauthorized action simulated, Splunk triggered an alert and Shuffle SOAR automatically disabled the compromised user account. Notifications were also sent through Telegram and Email to keep administrators informed. This project highlights how automation can improve AD security and streamline incident response.

### Skills Learned

- Active Directory Configuration: Gained hands-on experience setting up and managing a Windows-based Active Directory environment in a virtualized environment.
- SIEM Integration with Splunk: Learned how to collect, analyze, and create alerts from telemetry data using Splunk, specifically focused on Windows security events.
- Simulating Cyber Attacks: Developed skills in simulating RDP access to understand attack patterns and how they appear in logs.
- Security Automation using Shuffle SOAR: Installed and configured Shuffle SOAR on Ubuntu,Implemented automated incident response workflows including disabling compromised domain users and sending real-time notifications.
- Telegram bot creation and integration: Learned how to create bot and integrate it with Shuffle.
- Containerization with Docker and Docker Compose: Installed and utilized Docker and Docker Compose to manage containerized applications, including Shuffle SOAR.
- Virtualization and Network Lab Setup: Built and managed a multi-machine virtual lab using VirtualBox to simulate an enterprise-like network environment and learned how to set static ip in both windows and linux.

  ### Tools Used

-  VirtualBox – To create and manage virtual machines for simulating a networked lab environment.

-  Windows Server (Active Directory) – To configure and manage domain users.

-  Windows 10 Enterpriser - Acts as test machine(user of domain).

-  Ubuntu (Attacking Machine) – Used to simulate unauthorized RDP access attempts to the Active Directory environment.

-  Splunk – For collecting, analyzing, and alerting on telemetry data from the Active Directory system.

- Shuffle SOAR – To automate incident response workflows based on alerts from Splunk.

- Docker & Docker Compose – To install and manage Shuffle SOAR and related services in a containerized environment on Ubuntu.

- Telegram Bot – Integrated with Shuffle SOAR for real-time alert notifications and response confirmations.

- Webhook (HTTP Trigger) – Used for communication between Splunk alerts and the Shuffle SOAR platform.

  ### Project Map

  ![Project_ad_final1](https://github.com/user-attachments/assets/8b26bbfb-2d0a-41cc-9cdd-3f8ea76058ec)

  ### STEP BY STEP IMPLEMENTATION
  ### Phase 1: Lab Environment Setup:
  
 # Step 1: Setup Laptop 1 – Active Directory (Windows Server VM And Windows enterpriser VM):

Download and install the virtual box first and download the windows server by using this website https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025
and install the downloaded windows server on virtual box , Promote the server to a Domain Controller via Active Directory Domain Services , need to install both Active Directory Domain services and Active Directory certification services by using server manager.I have set my domain name as MARVEL.local, and .Following image shows you the confirmation about services installation :

![AD and Certificate installation confirmation](https://github.com/user-attachments/assets/11fb88e6-4ece-4d86-bf94-c854f90d8003)

I have set static ip(192.168.31.66) for my windows server virtual machine:

![Static ip(net_adapter)](https://github.com/user-attachments/assets/5b082368-484b-4ec6-a88f-6b4f91f03470)

Created user TonyStark, with the Tstark as username, you can confirm it with the following image:

![User TONYSTARK Added](https://github.com/user-attachments/assets/1eb0dcd5-0be9-4197-997f-8814e956e3ff)

Now download the Windows 10 Enterpriser (which acts is a test machine or user of the domain) by using this link: https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise , Install this in virtual box and join the domain with the same credentilas created at Domain server , by refering following image you can confirm that user TonyStark have joined MARVEL.local Domain:

![joined DC(controlpanel)](https://github.com/user-attachments/assets/ed84236b-6b35-446e-ac49-4b08a41f6155)

Need to set Domain Controller IP as User DNS , which ensures that proper AD functionality :

![joined DC(networkadapter)](https://github.com/user-attachments/assets/e3bf4b73-8caa-4012-903f-3a373c5fb415)

I have changed some details in the Test machine , which may helps to confirm that Testmachine successfully joined in active directory:

![user machine details](https://github.com/user-attachments/assets/08344545-ef90-45e9-a575-d139e99ede27)

You can confirm that Testmachine has joined in Domain by following image:

![computers(useradded)](https://github.com/user-attachments/assets/1f4ed5ff-9120-4af0-b7af-3b3ba5e2d603)

Note: I have enabled Remote Desktop in TestMachine

### Step 2: Setup Laptop 2 – Splunk, SOAR, and Attack Simulation (Ubuntu VM)

Downloaded and Installed ubuntu VM on virtual box, I have installed splunk on ubuntu VM:

![Screenshot from 2025-06-05 15-30-47](https://github.com/user-attachments/assets/6c129843-8446-4c7b-827a-a0c0d707aabb)

I have Configured index with the name "project-ad" and new receiving port as "9997" in splunk and installed Splunk Add-on for Microsoft Windows , it is important when you want to collect and properly interpret Windows data into splunk, without WIndows add-on splunk will not know how to collect, parse, or understand windows logs correctly . It enables full visibility and analysis of your environment.

![Screenshot from 2025-06-05 15-44-48](https://github.com/user-attachments/assets/38c6d9d0-f8c7-486c-b48d-307fbb72ec9a)

Done with the Downloading and installation of splunk universal forwarder on both the Domain controller and Domain user:

![splunk forwarder installed](https://github.com/user-attachments/assets/e6b5b0a5-daea-4934-97b4-50b5117dff89)

Note: I have added some details in the input.conf

                  [WinEventLog://Security]
                  index = project-ad
                  disable = false
To confirm forwarder state , we can confirm through the services running on the machine:

![splunk forwarder (services)](https://github.com/user-attachments/assets/28f9127d-2b5b-4dac-aecc-8b52fc37dae2)

I successfully got data to the splunk from both the machines after completion of splunk universal forwarder installation and configuration:

![Screenshot from 2025-06-05 21-36-51](https://github.com/user-attachments/assets/5ac1be08-9ab0-43ef-ae05-b3458d3f141c)

Shuffle SOAR requires Docker and Docker Compose because it runs as multiple connected services (like web UI, backend, and database) in containers. Docker handles running these services, and Docker Compose manages and starts them together with one simple command.
To install them I followed these steps: https://github.com/Venkatadri-Ch/Active-Directory-security-with-SIEM-SOAR/blob/2fcae1d06267e7d4504c77f9831df9684ffcb930/Installing-docker-%26-docker-compose.md

NOTE: Installing Shuffle SOAR on-premises requires more configuration than using the cloud version, like DNS configuration,Shuffle-worker setup, Set up SMTP settings for alerts and notifications, etc

To install Shuffle SOAR on ubuntu,I have followed these steps: https://github.com/Venkatadri-Ch/Active-Directory-security-with-SIEM-SOAR/blob/2fcae1d06267e7d4504c77f9831df9684ffcb930/Installing%20Shuffle%20SOAR.md

![Screenshot from 2025-06-11 10-31-50](https://github.com/user-attachments/assets/fe1ad854-f086-4c5b-b1e0-f65ac723683a)


NOTE: BY using BotFather in telegram , I have created one bot in telegram , which further used for alerting purpose

  ### Phase 2: Simulated Attack & Detection

### Step 3: Simulate Unauthorized RDP Access:

Installe dFreeRDP on ubuntu

    sudo apt update
    sudo apt install freerdp2-x11
    
This installs the FreeRDP client which provides the xfreerdp command.

Command syntax that I have used to gain access to Testmachine from ubuntu is: 

    xfreerdp /u:<username> /p:<password> /v:<ip_or_hostname>  /cert-ignore
    
After execution of the command I got access to the testmachine:

![Screenshot from 2025-06-06 11-32-43](https://github.com/user-attachments/assets/43b3fa85-ce6d-4779-aa76-f6bf6d1be8ea)

### Step 4: Detection of event and Creation of Alert in splunk

I detected a relevant event in Splunk that indicated potential remote desktop (RDP) activity.
Note: 192.168.31.180 = Domain User
      192.168.31.66 = Domain Controller

![Screenshot from 2025-06-17 08-07-43](https://github.com/user-attachments/assets/4d99d55a-6a25-4355-84bc-b408bd657776)

After analyzing the event details and confirming its significance, I proceeded to create a Splunk alert(with the name Unaitherized-successful-login-RDP) to monitor similar events in the future. This alert is configured to trigger whenever a matching pattern is detected, ensuring real-time visibility and proactive response.

![Screenshot from 2025-06-06 11-15-51](https://github.com/user-attachments/assets/5470704a-6ca8-4ac3-ac6c-9030b3c9fe57)

The Splunk alert was triggered as a result of detecting an event that matched the specified search criteria. This confirms that the alerting logic is correctly identifying the relevant RDP activity.

![Screenshot from 2025-06-06 11-18-33](https://github.com/user-attachments/assets/01f725b1-c202-46ab-b283-f229a6f132e8)


### Phase 3: Response Automation via Shuffle SOAR

### step 5: Configured Shuffle SOAR Workflow:

I’ve set up a new workflow in Shuffle SOAR called Project-AD. It’s focused on handling events related to Active Directory activities, like unautherized RDP logins or sucessfull logins, and it’s integrated with Splunk to automate part of the detection and response process.

A webhook trigger has been integrated into the Project-AD workflow within Shuffle SOAR. The webhook URL was copied from Shuffle and configured in a Splunk alert using the webhook alert action. This enables real-time communication from Splunk to Shuffle—when the alert conditions are met, Splunk sends an HTTP POST request to the webhook endpoint, thereby triggering the automation flow defined in Shuffle.

![Screenshot from 2025-06-06 11-48-31](https://github.com/user-attachments/assets/381aabe6-84eb-45f3-8e22-265841c6b737)

I had already set up a Telegram bot earlier, and I’ve now integrated it into the Shuffle workflow. I used the Telegram Bot app in Shuffle and connected it with my existing bot. After re-running the workflow, it successfully sent an alert message to the Telegram group, so the setup is working.

![Screenshot from 2025-06-16 10-33-40](https://github.com/user-attachments/assets/9e54f034-c64b-4c93-acda-17531034a9a4)

I enabled the Active Directory app in the Shuffle workflow and used the "Get User Attributes" action instead of disabling the user—just to test if the integration works. After re-running the workflow, it pulled the user info successfully, so the AD setup is confirmed to be working.

![Screenshot from 2025-06-11 19-43-15](https://github.com/user-attachments/assets/d69d32a3-331a-4a3e-ae88-5c03dd40ce27)

I’ve added the User Input app into the workflow so it now waits for a user decision. If the user clicks "Yes", the workflow continues. If "No" is selected, it stops. This gives us more control over which actions actually get executed, especially for sensitive operations.

<img width="608" alt="user input app" src="https://github.com/user-attachments/assets/de15a9a6-310b-4525-bec6-3b1157a4402f" />

It will sends mail aler like the follwing picture:

<img width="455" alt="mail alert" src="https://github.com/user-attachments/assets/1b44acc5-24c9-4af9-970c-b38738cbb5ba" />

when we select the option (YES/NO), it will pop up like this :

<img width="644" alt="userinput" src="https://github.com/user-attachments/assets/706bf68f-ff12-479a-8c03-326e9c7d5cba" />

I was kept the User input app aside for some time , to build the complete workflow without user action , making workflow as self automated without user input.

The current workflow begins with a Webhook trigger, which activates upon receiving an alert from Splunk. This is followed by a Telegram Bot notification, which sends the alert to the appropriate Telegram chat. Next, the Active Directory app is used to disable the user account associated with the alert. After that,another Active Directory app is executed to retrieve the user's attributes for further analysis and finally,I have utilizes the Shuffle tool to complete or manage the remaining actions. 

![Screenshot from 2025-06-16 12-56-51](https://github.com/user-attachments/assets/569417a5-c164-4e13-93e0-6dc5da3595b2)


In the output of workflow, I found that "ACCOUNTDISABLED"  :

![Screenshot from 2025-06-16 13-23-20](https://github.com/user-attachments/assets/8885152f-3438-4bd5-96f1-2eae85b64650)

After the Shuffle step(repeat back to me), a conditional check is applied: if the retrieved user attributes contain ACCOUNTDISABLED, the workflow proceeds to trigger a second Telegram Bot notification to confirm that the user's account has been successfully disabled. If the condition is not met, the Telegram update is skipped.

![Screenshot from 2025-06-16 13-34-52](https://github.com/user-attachments/assets/a053cc93-22ab-4608-8886-fa96503735f4)

This workflow is designed to operate completely autonomously, enabling rapid detection, response, and communication without requiring user approval or input. The User Input app has been intentionally excluded at this stage to streamline automation.

![Screenshot from 2025-06-16 13-54-14](https://github.com/user-attachments/assets/48988cf7-578e-4926-81d0-367163ab22a1)

Complete Workflow with the user input:

![Screenshot from 2025-06-16 15-00-01](https://github.com/user-attachments/assets/ff549619-3934-4fdb-aa4b-21bee7bea517)

### Final Workflow Overview with User Input App Integration

1. Trigger

- Source: Splunk

- Action: Webhook is triggered by a security alert.

2. Initial Notification

- Tool: Telegram Bot (Step 1)

- Action: Sends an alert to the security team detailing the detected event.

3. User Decision Point

- App: User Input

- Prompt: Asks the analyst whether to proceed with disabling the user account.

If "Yes" → Continue to next step.

If "No" → Workflow halts here.

4. User Account Action

- App: Active Directory (Step 1)

- Action: Disables the affected user account.

5. Attribute Retrieval

- App: Active Directory (Step 2)

- Action: Retrieves user attributes (e.g., status, flags) for confirmation and logging.

6. Post-Processing

- Tool: Shuffle tools (repeat back me)

- Action: Handles final data processing, normalization, or enrichment tasks.

7. Condition Check

- Condition: Checks if ACCOUNTDISABLED is present in the retrieved attributes.

If true → Proceed to final step.

If false → Skip notification.

8. Final Notification

- Tool: Telegram Bot (Step 2)

- Action: Sends confirmation that the user account has been successfully disabled.

### Phase 4: Notification & Confirmation
This phase ensures that appropriate alerts are sent to the security team and final actions are confirmed after execution. It includes initial alerting, validation of notification delivery, and a final status confirmation once the user account has been processed.

# Step 6: Alert Notifications Monitoring

# Telegram Bot Notification
Once the Splunk-triggered webhook activates the workflow, the first Telegram Bot step sends a real-time alert to the designated Telegram channel to inform the security team of the detected event.

<img width="577" alt="1st telegram" src="https://github.com/user-attachments/assets/37f1d043-0c52-4d04-b454-d5d1c9684b4e" />

# Email Alert
This will done in the workflow with user_input app , These notifications serve as the initial response indicators, alerting analysts to potential issues and prompting a manual decision through the User Input app.

<img width="455" alt="mail alert" src="https://github.com/user-attachments/assets/4cb49337-4d7c-4a1d-8c69-317765a128a1" />


# Step 7: Action Confirmation
After the user decision is made and, if approved, the user account has been disabled, the workflow continues to its final communication step.

# Final Telegram Confirmation
A second Telegram Bot action is triggered only after a successful account disablement is verified through attribute checks (e.g., detecting the ACCOUNTDISABLED flag in Active Directory). This message provides a clear confirmation to the team that remediation has been executed successfully.

<img width="956" alt="telegram full" src="https://github.com/user-attachments/assets/44dfb425-fe69-4c2e-b885-9a382bfa425b" />

### Step 8: Domain Controller Verification (Manual Check)

To ensure complete accuracy, a final manual confirmation is performed directly on the Domain Controller.Confirmed that user has been disabled:

![User DIsbled confirmation](https://github.com/user-attachments/assets/a32b0c9b-33c8-4e5a-b534-c6801b79fe2d)

Important Note: Disabling a user account does not terminate existing sessions. If the user is already logged in, the session remains active until they log out or the session is forcibly terminated.This is because we don't have logout feature in shuffle which may come in futute(their documentation said that). Here some manual work needed to log off the user i have used these simple steps:
Run this command in Domain Controller first to list active sessions:
    
     query session
output:

    SESSIONNAME       USERNAME        ID    STATE     TYPE        DEVICE
    rdp-tcp#4         testuser        2     Active    rdpwd
    console           Administrator   1     Active
To log off testuser, run:

    logoff 2
This will:

- Kill that RDP session

- Log the user out immediately

- Close all their open programs without warning

When I try to login after all these steps,I found this:

![IMG_20250620_115549](https://github.com/user-attachments/assets/afa65bdc-2bf5-4518-a1db-1e1ae08f4ee1)

###  Final Outcome

* RDP login attempt detected by Splunk and triggers alert.
* Alert triggers a webhook to Shuffle SOAR.
* Shuffle sends alert to Telegram_bot .
* User is automatically disabled in Active DirectoryAfter successful execution of Shuffle workflow.
* Updated Telegram notification confirms action to the admin or security team.
















































  



