<h1>Group Policy (GPO) Management Lab</h1>
<p>
Following the automated provisioning of a corporate Active Directory environment, this project focuses on endpoint management, user experience standardization, and security hardening using Group Policy Objects (GPOs).

</p>
The objective was to design, deploy, and verify 10 distinct policies across both User and Computer configurations. By linking these policies to specific Organizational Units (OUs), the lab successfully simulated a real-world deployment of corporate security baselines. This ensured that all domain-joined workstations adhered to baseline security standards while automatically provisioning necessary resources for end-users, ultimately reducing manual help desk interventions. <br />


<h2>Environments and Technologies Used</h2>

* **Infrastructure:** Windows Server 2022 Domain Controller, Windows 11 Client Workstation
* **Management Tools:** Group Policy Management Console (GPMC), Active Directory Users and Computers (ADUC)
* **Core Concepts:** Endpoint Hardening, Policy Inheritance, User Experience Standardization, Security Auditing

<h2>The Setup</h2>
<p>
  
Applying Group Policy Objects (GPOs) allows for another level of control when administering our corporate environment I set up in my Active Directory [lab](https://github.com/simeonjackson/ad-troubleshooting). Instead of manually logging into each computer on my domain and mapping network drives or setting wallpapers, I can use GPOs to create a set of configurations that will automatically apply.

</p>
In this lab I came up with 10 different GPOs to target both user behaviors and machine-level settings.


<h2>The Setup</h2>

This deployment is split into two categories:

**User Configuration:** These settings follow the person and apply at logon. <br />
**Computer Configuration:** These settings follow the hardware and apply at boot.

And here are the ten plicies impleneted in this lab:

**User Configuration Policies:**
* Mapped corporate network drives (`\\hq-fileserver\General`) automatically.
* Enforced a 10-minute screen saver timeout requiring a password to unlock.
* Blocked standard user access to the Command Prompt (`cmd.exe`).
* Prohibited access to the Control Panel and Windows PC Settings.
* Forced a standardized corporate desktop wallpaper.

**Computer Configuration Policies:**
* Configured an interactive legal notice/warning banner prior to user logon.
* Enabled auditing for all successful and failed logon events for security tracking.
* Renamed the built-in local Administrator account to mitigate brute-force attacks.
* Enforced Windows Defender Firewall to an "Always On" state.
* Configured automatic Windows Updates (Download but Notify to Install) to prevent unexpected midday reboots.

Below I will go through the configuration and deployment of each policy, verifying it works and giving my objective behind setting the policy.

<h2>Deployment and Configuration Steps</h2>

<h2>User Configuration Policies</h2>

<h3>- Mapping Network Drives</h3>

<p>
In this policy, I automatically mapped the General corporate file share for all users in my Headquarters OU. It is common fior an enterprise to have a share folder on all devices from which employees will access resources. This policy will ensure this share is available to all of our users once they verify their identity and log in.

I configured a User Configuration Preference to map `\\hq-fileserver\General` to the `G:\` drive using the "Replace" action to ensure the drive maps consistently upon every logon.

<img width="1501" height="856" alt="Image" src="https://github.com/user-attachments/assets/4376f724-818d-4bbb-ba54-342d802ed275" />

<img width="1508" height="857" alt="Image" src="https://github.com/user-attachments/assets/63c20577-4833-4ef8-81df-0e580bfcd0e7" />

<img width="1497" height="857" alt="Image" src="https://github.com/user-attachments/assets/092a15f8-05d9-4231-9cab-ef0b87cd8c6b" />

<img width="1513" height="901" alt="Image" src="https://github.com/user-attachments/assets/71210012-f500-4c34-97ca-c700afcf6a35" />

I learned after some issues that setting Action to "Replace" instead of "Update" ensures the drive maps consistently every time the user logs in.

<img width="1514" height="889" alt="Image" src="https://github.com/user-attachments/assets/de8ce6af-b0b8-45c9-babf-6c2aff450402" />

<img width="1543" height="910" alt="Image" src="https://github.com/user-attachments/assets/09f7a6ba-5ef7-4f2c-96cd-25b3511a6c71" />

To verify this worked, I logged into the client VM as a standard user (Ana Salas). Opened File Explorer and verified the `G:\` drive was present, fully accessible, and mapped to the correct server path without manual intervention.
</p>

<img width="1536" height="945" alt="Image" src="https://github.com/user-attachments/assets/caa62006-a83d-4da0-8d48-3f3bf21af6b7" />

<img width="1423" height="929" alt="Image" src="https://github.com/user-attachments/assets/3a67cc89-aaf8-45e0-acb5-1f8ba9e6c0fc" />

<h3>- Screen Saver Timeout with Password</h3>

<p>

</p>
