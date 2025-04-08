<h1>Group Policy Objects</h1>
This tutorial outlines the implementation of a Group Policy Object (GPO) within our domain where I create an account lockout policy.  <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure
- Group Policy Management
- Active Directory
- Remote Desktop

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

![Screenshot 2025-03-26 213804](https://github.com/user-attachments/assets/dcd6a9f6-4fad-43c9-a7cb-a9eb57193adc)


<p>
Under my domain there is a directory called Group Policy Objects (GPO). There are already a few policies created and I added another policy called Account Lockout Policy.

Right clicking and editing my new policy brings up the Group Policy Management Editor.
</p>

![Screenshot 2025-03-26 214305](https://github.com/user-attachments/assets/83894458-33de-4afb-bdce-6859705d1ff6)


<p>
Expanding the following directories under the Group Policy Management Editor will lead you to the account policy settings where you will find lock out settings.

Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy
</p>

![Screenshot 2025-03-26 214448](https://github.com/user-attachments/assets/f0f3f66c-bead-4196-8e96-1568b0233362)


<p>
There are four settings that I can configure for this policy:

Account Lockout Duration - Defines the time an account is locked out for after hitting lockout threshold
Account Lockout Threshold - Defines the number of failed log on attempts allowed before account lockout
Allow Administrator Account Lockout - This will allow an administrator account to be locked out due to failed attempts
Reset Account Lockout Counter - This defines the time after which the failed logon attempts counter is reset to zero

For my policy I set the lockout duration to 30 minutes, lockout threshold to 5 attempts and set the reset counter to 10 minutes. I left the setting for administrator lock out to enabled which is a good security practice.
</p>

![Screenshot 2025-03-26 214829](https://github.com/user-attachments/assets/e8bd1dd6-d6d8-428c-91f3-76cb2dc65c66)


<p>
Highlighting our new policy and going to settings we can see what we just configured.
</p>

![Screenshot 2025-03-26 215106](https://github.com/user-attachments/assets/8504b4f5-e15f-4a2e-b275-5263892ad3d9)


<p>
To link the GPO once it is configured, you have to link it to a domain or organizational unit (OU) which can be done by right clicking on the OU or domain and selecting Link an Existing GPO.

</p>

![Screenshot 2025-03-26 215323](https://github.com/user-attachments/assets/bfe3a0df-3799-48d1-b970-90470ba56bdb)


<p>
This policy will update automatically after 90 minutes. In order to push through the change right away you can use the following command to force the update right away.

	gpupdate /force
</p>

![Screenshot 2025-03-26 220224](https://github.com/user-attachments/assets/3d6990ee-699c-47e7-9138-3c85be5ec911)


<p>
In order to test that our policy was made correctly, I will attempt to log in as John Garcia on my domain failing five log-in attempts to lock out the account.
</p>

![Screenshot 2025-03-26 220827](https://github.com/user-attachments/assets/4111d5db-24b0-4e0e-b94a-142c364d15b7)


<p>
As an administrator, I will find our user in Active Directory where I can verify that the account is locked out. I reset the users password and unlocked the account. I also checked the box to prompt the user to change their password on there next logon.
</p>

![Screenshot 2025-03-26 221223](https://github.com/user-attachments/assets/90d8c570-e83e-44c1-956f-17fed631752a)


<p>
Attempting to log in with our user again, this time using the correct login, I can see that the account is unlocked and I am able to access my client machine.
</p>

![Screenshot 2025-03-26 221825](https://github.com/user-attachments/assets/dca7121e-369b-48f9-ae83-233dd9c11a3d)


<p>
And there you have it. This is a basic overview of one of the ways GPOs can be used to strengthen an organization's security.
</p>
