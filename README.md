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


  



