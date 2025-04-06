<h1>Group Policy Objects</h1>
This tutorial outlines the implementation of a Group Policy Object (GPO) within our domain where I create an account lockout policy.  <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure
- Group Policy Management
- Active Directory
- Remote Deskto[

<h2>Operating Systems Used </h2>

- Windows 11
- Windows Server 2022

<h2>Deployment and Configuration Steps</h2>

<p>
Configuring an account lockout policy in Active Directory using Group Policy involves defining settings that control when an account is locked after multiple failed login attempts, how long the account remains locked, and how the lockout counter is reset.

I make a Group Policy Object in this lab that will help an administrator set and control parameters that would cause a user account to be locked out. This is a security practice that could prevent brute-force attacks on your network.

This scratches the surface of what Group Policy can do for an organization. GPOs can be used for password policies, automatic updates, security groups, and much more.

I logged into my domain controller and opened Group Policy Management Console (GPMC) to get started.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Under my domain there is a directory called Group Policy Objects (GPO). There are already a few policies created and I added another policy called Account Lockout Policy.

Right clicking and editing my new policy brings up the Group Policy Management Editor.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Expanding the following directories under the Group Policy Management Editor will lead you to the account policy settings where you will find lock out settings.

Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
There are four settings that I can configure for this policy:

Account Lockout Duration - Defines the time an account is locked out for after hitting lockout threshold
Account Lockout Threshold - Defines the number of failed log on attempts allowed before account lockout
Allow Administrator Account Lockout - This will allow an administrator account to be locked out due to failed attempts
Reset Account Lockout Counter - This defines the time after which the failed logon attempts counter is reset to zero

For my policy I set the lockout duration to 30 minutes, lockout threshold to 5 attempts and set the reset counter to 10 minutes. I left the setting for administrator lock out to enabled which is a good security practice.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Highlighting our new policy and going to settings we can see what we just configured.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


<p>
To link the GPO once it is configured, you have to link it to a domain or organizational unit (OU) which can be done by right clicking on the OU or domain and selecting Link an Existing GPO.

</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
This policy will update automatically after 90 minutes. In order to push through the change right away you can use the following command to force the update right away.

	gpupdate /force
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
In order to test that our policy was made correctly, I will attempt to log in as John Garcia on my domain failing five log-in attempts to lock out the account.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
As an administrator, I will find our user in Active Directory where I can verify that the account is locked out. I reset the users password and unlocked the account. I also checked the box to prompt the user to change their password on there next logon.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Attempting to log in with our user again, this time using the correct login, I can see that the account is unlocked and I am able to access my client machine.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
And there you have it. This is a basic overview of one of the ways GPOs can be used to strengthen an organization's security.
</p>
