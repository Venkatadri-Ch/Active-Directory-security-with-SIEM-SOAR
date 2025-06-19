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

To install Shuffle SOAR on ubuntu,I have followed these steps: https://github.com/Venkatadri-Ch/Active-Directory-security-with-SIEM-SOAR/blob/2fcae1d06267e7d4504c77f9831df9684ffcb930/Installing%20Shuffle%20SOAR.md


























  



