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
With this policy, the goal is to secure unattended workstations to prevent unauthroized ohysical access to corporate data. To do this I enabled two settings "Screen Saver Timeout" and "Password Protect the Screen Saver"

<img width="1385" height="759" alt="Image" src="https://github.com/user-attachments/assets/ce18a780-7824-4ab3-8455-c527a8c48465" />

<img width="1284" height="703" alt="Image" src="https://github.com/user-attachments/assets/e4bdff2c-3d70-4c82-843f-eee1927f53e6" />

For the "Screen Saver Timeout" I enable the setting and set the timer to 2 minutes, of inactivity before the screen saver triggers.
<img width="1278" height="769" alt="Image" src="https://github.com/user-attachments/assets/e16ab1c5-cac1-4ff7-8f9a-ce06fc504d0a" />
</p>

Triggering a screen saver is not enough to prevent unauthorized access. I also enable a setting that will password protect the screen saver. Meaning once triggered , the only way to gain access is to re-enter the correct password for the current user.

<img width="1285" height="710" alt="Image" src="https://github.com/user-attachments/assets/53caed2c-23c6-4135-9626-611b0078e789" />

<img width="1917" height="1079" alt="Image" src="https://github.com/user-attachments/assets/b1a98b21-9466-40f5-8013-2e5e408b7edd" />

<h3>- Block Command Prompt Access</h3>

<p>
Here I am blocking standard users from using the Command Prompt. This will ensure that running unauthorized scripts and network discovery commands will be prevented unless strict access is granted.

To start, I enabled "Prevent access to the command prompt" under `User Configuration > Administrative Templates > System`.
<img width="1253" height="788" alt="Image" src="https://github.com/user-attachments/assets/afa4c868-2d37-4dd4-924e-58c62f15abe6" />

<img width="1428" height="767" alt="Image" src="https://github.com/user-attachments/assets/034c6ad5-a980-40ab-9344-15bdad0d4e05" />

This GPO is currently applied to all Authenticated Users including admins within the domain, meaning admins cannot access the Command Prompt. This can be changed by locating the Delegation tab while looking at the GPO. Clicking Advanced, and setting the Domain Admins permission Apply Group Policy to Deny.
<img width="1428" height="701" alt="Image" src="https://github.com/user-attachments/assets/2acff442-3076-4c75-87a5-c5a9393ca5f8" />

<img width="1330" height="726" alt="Image" src="https://github.com/user-attachments/assets/6a853568-0279-4ffa-9088-93ad600a602f" />


Checking a user account to see if the policy applied correctly and it has.
<img width="975" height="514" alt="Image" src="https://github.com/user-attachments/assets/314519af-d4b9-4ef0-82a6-8bb8851ecbf3" />

</p>

<h3>- Prohibit Control Panel and PC Settings Access</h3>

<p>
In the same vein as preventing Command Prmpt access, I do not want end-users to be able to alter system configurations, uninstall critical software, or access many of the feautes found under the Control Panel or PC Settings. So I will add "Prohibit access to Control Panel and PC Settings" to the GPO that I just made.
  <img width="1207" height="785" alt="Image" src="https://github.com/user-attachments/assets/811903fb-f87f-4a02-8090-e9ea676b47d2" />

Here I confirm that access is prohibited as a standard user.
<img width="1332" height="797" alt="Image" src="https://github.com/user-attachments/assets/ade7caf7-e0e3-45e2-a4e6-2a97e26b9be3" />

</p>

<h3>- Desktop Wallpaper</h3>

<p>
In this policy I set a standard desktop wallpaper that all of my users will get when logging on. In a corporate environment this can be used for uniformity or to prevent users from setting inappropriate backgrounds.

  I downloaded a generic image and saved it to my general file share that all my users have access to. I made sure to set it to read-only so that it cannot be modified or deleted.
<img width="1035" height="667" alt="Image" src="https://github.com/user-attachments/assets/6809fba2-87fb-4e30-8421-83b330ea06e1" />

Enabling "Desktop Wallpaper" under `User Configuration > Administrative Templates > Desktop > Desktop`, pointing to my image share (`\\hq-fileserver\General\Wallpaper.jpg`).
<img width="1711" height="942" alt="Image" src="https://github.com/user-attachments/assets/fd2fc8cd-ec96-4677-b72b-f975c78881f9" />

I also enabled the setting "Prevent changing desktop background".
<img width="1588" height="940" alt="Image" src="https://github.com/user-attachments/assets/55221a4d-bebb-47a5-b80e-c6b1a962b963" />

When logging into my client VM as any user I now have my corporate wallpaper set.
<img width="1917" height="1077" alt="Image" src="https://github.com/user-attachments/assets/6adf5735-0ad5-4e99-a39d-355413df1f6e" />

</p>

<h2>Computer Configuration Policies</h2>

<h3>- Adding Logon Legal Notice</h3>

<p>

</p>

<h3>- Audit Logon Events</h3>

<p>

</p>

<h3>- Renaming Built-In Admin Account</h3>

<p>

</p>

<h3>- Enabling Windows Defender Firewall</h3>

<p>

</p>

<h3>- Automatic Updates (Notify to Install)</h3>

<p>

</p>

