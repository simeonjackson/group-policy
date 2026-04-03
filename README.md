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


<h2>User vs. Computer Configuration</h2>

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
This first computer configuration policy is generating a legal warning banner upon boot before a user starts a session. Users will have to consent to having read the message before being able to logon.

A small troubleshooting error that I ran into here was not removing the Client VM from its default Computers container and putting it in an OU. This has to be done because policies only apply to objects inside of OUs.
<img width="1230" height="823" alt="Image" src="https://github.com/user-attachments/assets/ff02c498-2bae-42a4-aa99-3f6147b35210" />

I found "Interactive logon:..." under `Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options` and set a policy title and text.
<img width="1651" height="916" alt="Image" src="https://github.com/user-attachments/assets/eab3aa6b-05b8-4047-b8a9-5c9a5bf584cd" />

<img width="1279" height="847" alt="Image" src="https://github.com/user-attachments/assets/9ebe5911-faea-473a-9176-3c6b0d6d9e04" />

<img width="1276" height="853" alt="Image" src="https://github.com/user-attachments/assets/1aade289-93df-459f-bce9-9d35855e366b" />

Here is the final result displayed upon boot before the login screen.
<img width="1918" height="1077" alt="Image" src="https://github.com/user-attachments/assets/17248e77-e96b-4656-9038-65a0e9ae1a79" />
<img width="1908" height="1075" alt="image" src="https://github.com/user-attachments/assets/f065e7cc-95cc-45c6-b42c-6d7a13d36300" />


</p>

<h3>- Audit Logon Events</h3>

<p>
I next created a policy that will ensure that all authentication attempts on domain machines are actively logged to monitor for unauthorized access attempts. These logs will be available to view in the Event Viewer and can help us to investigate suspicious logon activity.

I navigated to `Computer Configuration > Policies > Windows Settings > Security Settings > Advanced Audit Policy Configuration` and enabled Success/Failure logging for Logon events.

<img width="1747" height="952" alt="Image" src="https://github.com/user-attachments/assets/aeb16458-8ce7-4ef1-8442-fb6cbbc2b8f1" />

<img width="1546" height="853" alt="Image" src="https://github.com/user-attachments/assets/abf7dea3-b868-4238-8cbd-c21d3a8f034d" />

Intentionally failed a login attempt on the client workstation. Opened the Windows Event Viewer (Security Log) as an administrator and successfully located Event ID 4625 (Audit Failure), confirming the environment was successfully tracking security events.

<img width="1912" height="1018" alt="Image" src="https://github.com/user-attachments/assets/01382a81-e9b0-4803-ac55-2befc44e085a" />

<img width="1569" height="988" alt="Image" src="https://github.com/user-attachments/assets/9c444e96-a453-4320-a38f-2573696d9d44" />
</p>

<h3>- Renaming Built-In Admin Account</h3>

<p>
Every single Windows computer ever manufactured comes with a built-in local account named exactly `Administrator`. This account is the "break glass" emergency account, and it has absolute, unrestricted power over that physical hardware. Because this is a universal standard, every automated ransomware script, brute-force hacking tool, and malicious bot on the internet is programmed to knock on the front door and ask for the user "Administrator." In the security world, an attacker needs two things to break in: a username and a password. By leaving the default name, you are giving them 50% of the puzzle for free.

The fix for this is simple, I configured "Accounts: Rename administrator account" under `Computer Configuration > Windows Settings > Security Settings > Local Policies > Security Options`, changing the default name to an obscure standard (`LocalAdmin-IT` in this case).

<img width="1777" height="925" alt="Image" src="https://github.com/user-attachments/assets/27290bcd-612b-4d16-836c-f741b7b495d9" />

<img width="1381" height="778" alt="Image" src="https://github.com/user-attachments/assets/7690fe06-298c-47b1-b617-d53b90213692" />

I then opened Computer Management (`compmgmt.msc`) on the client VM, navigated to Local Users and Groups, and verified the default Administrator account was successfully renamed.

<img width="1480" height="996" alt="Image" src="https://github.com/user-attachments/assets/dd5205e6-3958-4cdd-b447-7eac5ad9b8a5" />
</p>

<h3>- Enabling Windows Defender Firewall</h3>

<p>
Here I am going to enforce Windows Defender Firewall into an "Always On" state. This will prevent users and potential attackers from disabling the local firewall ensuring endpoint protection remains active.

I set "Windows Defender Firewall: Protect all network connections" to Enabled under `Computer Configuration > Administrative Templates > Network > Network Connections > Windows Defender Firewall > Domain Profile`.
<img width="1771" height="919" alt="Image" src="https://github.com/user-attachments/assets/f1f7ae5a-428f-49ba-867a-53ca0747aed7" />

<img width="1023" height="954" alt="Image" src="https://github.com/user-attachments/assets/cb8e5930-a4ae-477b-b86c-e1d526dd54ef" />

To verify I logged in as a user to the client VM and verified that the firewall status was active and the option to turn it off was greyed out. It also stats that these settings are managed by the administrator.

<img width="1168" height="696" alt="Image" src="https://github.com/user-attachments/assets/779f36db-fe14-44e6-855c-68335b7f3e5a" />

</p>

<h3>- Automatic Updates (Notify to Install)</h3>

<p>
This policy will prevent servers or workstations from rebooting in the middle of a day due to automatic Windows updates. The Updates will download but not install allowing the the installation to be planned by an IT team during a period of downtime.

Configured "Configure Automatic Updates" to "Option 3 - Auto download and notify for install" under `Computer Configuration > Administrative Templates > Windows Components > Windows Update`.

<img width="1690" height="924" alt="Image" src="https://github.com/user-attachments/assets/6bf264a8-be02-45ac-90d0-1c1f73ab74f4" />

<img width="1137" height="955" alt="Image" src="https://github.com/user-attachments/assets/e920fc41-6f26-4e3a-9a8c-0030ba7131a2" />

<img width="1632" height="948" alt="Image" src="https://github.com/user-attachments/assets/544d76af-339a-4a7b-a8ae-cd2a707f0019" />

Pbviously it is important to install these patches onto our devices. This policy is telling our computers to download the patch but wait for explicit approval or a maintenance window before installing.

<h2>Takeaways</h2>

<p>
Running through some practical uses for Group Policy gave me some insight on how automation can be used to ensure consistency in an environment helping reduce human errors, time and help desk tickets. It also allows a proactive stance to be taken on certain security issues.

All in all, it is nice to see how these policies tie into Active Directory forming the basis of identity management, least privilege, and access control.
</p>
