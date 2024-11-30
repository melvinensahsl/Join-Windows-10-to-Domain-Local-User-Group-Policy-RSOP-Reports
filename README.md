<h1>Overview: Lab 5 Join Windows 10 to Domain (Local User), Group Policy, RSOP Reports</h1>

This section of my home lab documentation demonstrates the process of **joining a Windows 10 machine to a domain** with a **local user**, configuring **Group Policy** settings, and generating **RSOP (Resultant Set of Policy) reports** to analyze the applied policies. The project focuses on how to manage and enforce policies across computers in a domain using Group Policy Objects (GPOs) and review their application using RSOP reports. 

<h2>Objectives</h2>

- **Join a Windows 10 PC to a domain** with a local user account.
- Configure and apply **Group Policy Objects (GPOs)** to enforce settings on domain-joined computers.
- Generate **RSOP (Resultant Set of Policy)** reports to review and troubleshoot applied policies.
- Use **Group Policy Management Console (GPMC)** to manage and configure GPOs.


<h2>Documentation</h2>

 

In this home lab, we will create another virtual machine running **Windows 10**, named **Desktop2**, to simulate a local user account for testing purposes. This virtual machine will represent an employee in our lab environment. For example, if this user encounters password issues, we can reset it using the **Help Desk** account on our primary Windows 10 virtual machine.

To create the new virtual machine, click on **Machine** → **New**, then name it **Desktop2**. Select the **Windows** ISO image from your downloads, choose **Skip unattended installation**, and then click **Finish**.


1. <p align="center">
   <img src="https://i.imgur.com/Nq1RIC7.png" height="100%" width="100%" alt="Disk Sanitization Steps 1"/>
   <br />
   <br />
</p>

The process will be the same as we did for our other Windows 10 virtual machine for the Helpdesk account. Select Next → Install now → I don't have a product key.

2. <p align="center">
   <img src="https://i.imgur.com/6eRiZLX.png" height="100%" width="100%" alt="Disk Sanitization Steps 2"/>
   <br />
   <br />
</p>

Select Windows 10 Pro, then click Next → Custom: Install Windows only → Next.

3. <p align="center">
   <img src="https://i.imgur.com/krprDWm.png" height="100%" width="100%" alt="Disk Sanitization Steps 3"/>
   <br />
   <br />
</p>

Continue with the same configurations as before for the Windows 10 Helpdesk account. Select Personal Use, then enter User for the name and skip the password creation.

4. <p align="center">
   <img src="https://i.imgur.com/Db08KoK.png" height="100%" width="100%" alt="Disk Sanitization Steps 4"/>
   <br />
   <br />
</p>

Now that **Desktop2** is created, we can create a user for this computer. To do this, open up your **Helpdesk PC** virtual machine and sign in as **Helpdesk**. Keep in mind that **Windows Server 2022** needs to be running in the background to provide access to **Active Directory Users and Computers** on our **Desktop1** lab machine.

Remember, we currently have three virtual machines running: **Windows Server 2022**, **Windows 10 Helpdesk**, and our newly created **Windows 10 local user account**.

5. <p align="center">
   <img src="https://i.imgur.com/rf6QEdr.png" height="100%" width="100%" alt="Disk Sanitization Steps 5"/>
   <br />
   <br />
</p>

Let’s open Active Directory Users and Computers on the Windows 10 Helpdesk machine. Right-click on our domain SimoTech.com, then select New → Organizational Unit. Name the first Organizational Unit HR. Repeat the process and create a second Organizational Unit, naming it IT. You should now have two OUs: HR and IT.

6. <p align="center">
   <img src="https://i.imgur.com/vKGiwB7.png" height="100%" width="100%" alt="Disk Sanitization Steps 6"/>
   <br />
   <br />
</p>

Next, let’s create a user in Active Directory. Right-click on Users under the domain, then select New → User. Name the user Bob (first and last name) and set the Logon Name to Bob. Make sure all the options are unchecked, then set a password for Bob and click Finish to complete the user creation.

7. <p align="center">
   <img src="https://i.imgur.com/CRYfQ65.png" height="100%" width="100%" alt="Disk Sanitization Steps 7"/>
   <br />
   <br />
</p>

s

8. <p align="center">
   <img src="https://i.imgur.com/UHwNUBy.png" height="100%" width="100%" alt="Disk Sanitization Steps 8"/>
   <br />
   <br />
</p>

Now, let's drag the newly created user Bob into the HR organizational unit (OU). When the prompt appears asking if you are sure, click Yes to confirm the action. This will successfully move Bob into the HR OU.

9. <p align="center">
   <img src="https://i.imgur.com/PsRnr6X.png" height="100%" width="100%" alt="Disk Sanitization Steps 9"/>
   <br />
   <br />
</p>

Next, move the 'Helpdesk' user into the IT OU. After completing this, you should have 'Bob' in the HR OU and 'Helpdesk' in the IT OU. To verify the users' locations, enable 'Advanced Features' by selecting the 'View' tab at the top, then choosing 'Advanced Features'.

10. <p align="center">
   <img src="https://i.imgur.com/DVdteOw.png" height="100%" width="100%" alt="Disk Sanitization Steps 10"/>
   <br />
   <br />
</p>

Now, to locate the user 'Bob,' right-click on the domain SimoTech.com and select 'Find.' In the 'Find' window, right-click on 'Users' and choose 'Properties.' Then, select 'Entire Directory' and type 'Bob' in the search box. Double-click on his name when it appears, and navigate to the 'Objects' tab to view his details.

11. <p align="center">
   <img src="https://i.imgur.com/ni2Vu73.png" height="100%" width="100%" alt="Disk Sanitization Steps 11"/>
   <br />
   <br />
</p>

s

12. <p align="center">
   <img src="https://i.imgur.com/vGVYg1O.png" height="100%" width="100%" alt="Disk Sanitization Steps 12"/>
   <br />
   <br />
</p>

s

13. <p align="center">
   <img src="https://i.imgur.com/s1XWVJ4.png" height="100%" width="100%" alt="Disk Sanitization Steps 13"/>
   <br />
   <br />
</p>

In the Objects tab, you should see that Bob is part of the HR organizational unit, listed as SimoTech.com/HR/Bob.

14. <p align="center">
   <img src="https://i.imgur.com/dW5zIg0.png" height="100%" width="100%" alt="Disk Sanitization Steps 14"/>
   <br />
   <br />
</p>

We can confirm this with Helpdesk.

15. <p align="center">
   <img src="https://i.imgur.com/uHscmuL.png" height="100%" width="100%" alt="Disk Sanitization Steps 15"/>
   <br />
   <br />
</p>

Let's navigate to Group Policy Management in Server Manager using our Helpdesk account. From there, go to "Tools" and select "Group Policy Management."

16. <p align="center">
   <img src="https://i.imgur.com/F70ETsh.png" height="100%" width="100%" alt="Disk Sanitization Steps 16"/>
   <br />
   <br />
</p>

This will display the group policy for our domain controller. Select "Domains" → "SimoTech.com" → "Default Domain", then click "Generate Report." Next, go to "Settings" and click "Show All" in the top-right corner.

17. <p align="center">
   <img src="https://i.imgur.com/nsxbZgZ.png" height="100%" width="100%" alt="Disk Sanitization Steps 17"/>
   <br />
   <br />
</p>

This report offers a comprehensive view of various policies, including account policies, password policies, and account lockout policies. It is an invaluable tool for understanding the policies applied to a user. For instance, we can observe that the Account Lockout Policy is configured with a threshold of 0 invalid logon attempts. This setting poses a security risk, as it allows unlimited login attempts, making the system vulnerable to brute-force attacks.

18. <p align="center">
   <img src="https://i.imgur.com/u4iO7Cn.png" height="100%" width="100%" alt="Disk Sanitization Steps 18"/>
   <br />
   <br />
</p>

To modify this policy, right-click on "Default Domain Policy" and select "Edit."

19. <p align="center">
   <img src="https://i.imgur.com/lm1Z5g5.png" height="100%" width="100%" alt="Disk Sanitization Steps 19"/>
   <br />
   <br />
</p>

Select "Policy," navigate to "Windows Settings," then to "Security Settings," and double-click on "Account Policies."

20. <p align="center">
   <img src="https://i.imgur.com/3coajPR.png" height="100%" width="100%" alt="Disk Sanitization Steps 20"/>
   <br />
   <br />
</p>

Double-click "Account lockout duration," enable the "Define this policy setting" option, and set it to 30 minutes. Additionally, as a personal preference, you can modify the "Account lockout threshold" by double-clicking it and reducing the number of invalid logon attempts from 5 to 4.


21. <p align="center">
   <img src="https://i.imgur.com/dNu2n1w.png" height="100%" width="100%" alt="Disk Sanitization Steps 21"/>
   <br />
   <br />
</p>

s

22. <p align="center">
   <img src="https://i.imgur.com/o9u6k3r.png" height="100%" width="100%" alt="Disk Sanitization Steps 22"/>
   <br />
   <br />
</p>

Now, let's configure some settings in the Password Policy tab. Adjust the Maximum password age to 90 days.

23. <p align="center">
   <img src="https://i.imgur.com/Cl5uX3d.png" height="100%" width="100%" alt="Disk Sanitization Steps 24"/>
   <br />
   <br />
</p>

After configuring the policies, let's enforce them by right-clicking on "Default Domain Policy" and selecting "Enforced."

24 <p align="center">
   <img src="https://i.imgur.com/eHCGe6f.png" height="100%" width="100%" alt="Disk Sanitization Steps 25"/>
   <br />
   <br />
</p>

To verify that our policies have been updated, open Server Manager and navigate to Tools → Group Policy Management → Default Domain Policy. Generate a report, go to Settings, and select Show All. Confirm that all changes are enforced: the Maximum password age is set to 90 days, and the Account lockout threshold and Account lockout duration reflect the configurations we applied.

25. <p align="center">
   <img src="https://i.imgur.com/CctXfrX.png" height="100%" width="100%" alt="Disk Sanitization Steps 26"/>
   <br />
   <br />
</p>

Let's return to the Desktop2 virtual machine and open File Explorer. Right-click on This PC and select Properties. Click on Rename this PC (Advanced) → Change, update the current computer name to "Desktop2," and then click OK to apply the change. Finally, restart the system to complete the process.


26. <p align="center">
   <img src="https://i.imgur.com/2fWX2e8.png" height="100%" width="100%" alt="Disk Sanitization Steps 27"/>
   <br />
   <br />
</p>

After the restart, let's enable the Administrator account. Open File Explorer, right-click on This PC, and select Manage. In the Computer Management window, navigate to Local Users and Groups → Users. Right-click on Administrator, select Properties, and uncheck "Disable account". Then click Apply and OK to enable the account.

27. <p align="center">
   <img src="https://i.imgur.com/ZpSRmeF.png" height="100%" width="100%" alt="Disk Sanitization Steps 28"/>
   <br />
   <br />
</p>

Right-click on the Administrator account again and select Set Password. Enter the desired password for the account and confirm it. Then click OK to apply the changes.

28. <p align="center">
   <img src="https://i.imgur.com/0h9pIoK.png" height="100%" width="100%" alt="Disk Sanitization Steps 29"/>
   <br />
   <br />
</p>

After that, sign out of the PC and log into the administrator. 

29. <p align="center">
   <img src="https://i.imgur.com/Yjp9Qxn.png" height="100%" width="100%" alt="Disk Sanitization Steps 30"/>
   <br />
   <br />
</p>

To remove the **User** login screen, follow these steps:

1. Open **File Explorer** and right-click on **This PC**.
2. Select **Properties** and then click on **Advanced system settings**.
3. Under the **User Profiles** section, click **Settings**.
4. Find and select **Desktop\User** from the list, then click **Delete** to remove the user profile.

30. <p align="center">
   <img src="https://i.imgur.com/37hfhDj.png" height="100%" width="100%" alt="Disk Sanitization Steps 31"/>
   <br />
   <br />
</p>

Next, open **Control Panel** and go to **View Network Status and Tasks**. Click on **Change adapter settings**, then right-click on the **Ethernet** connection and select **Properties**.

Double-click on **Internet Protocol Version 4 (TCP/IPv4)**, and configure the static IP settings as follows:

- **IP Address:** 12.1.10.4
- **Subnet Mask:** 255.0.0.0
- **Default Gateway:** 12.1.10.1
- **Preferred DNS Server:** 12.1.10.2
- **Alternate DNS Server:** 12.1.10.1

Click **OK** to apply the settings.

31. <p align="center">
   <img src="https://i.imgur.com/NClFDlp.png" height="100%" width="100%" alt="Disk Sanitization Steps 32"/>
   <br />
   <br />
</p>

Next on our virtual machine, select “Devices” → “Network” → “Network settings” →  and change “NAT” to “Host-only Adapter”.

32. <p align="center">
   <img src="https://i.imgur.com/UJW30Em.png" height="100%" width="100%" alt="Disk Sanitization Steps 33"/>
   <br />
   <br />
</p>

Now lets open Command Prompt and ping our domain, SimoTech.com.

33. <p align="center">
   <img src="https://i.imgur.com/M4H3Bvl.png" height="100%" width="100%" alt="Disk Sanitization Steps 34"/>
   <br />
   <br />
</p>

Now, let's add this PC to our domain by opening File Explorer, right-clicking on This PC, selecting Properties, then clicking on Rename this PC (Advanced), and finally selecting Change.

34. <p align="center">
   <img src="https://i.imgur.com/G7GEc5B.png" height="100%" width="100%" alt="Disk Sanitization Steps 35"/>
   <br />
   <br />
</p>

Then, we can use our Helpdesk administrator account to join the domain. Afterward, restart the VirtualBox. Once restarted, check Active Directory Users and Computers → Computers under our Helpdesk account. You should see that Desktop2 has been successfully added to our domain, SimoTech.com.

35. <p align="center">
   <img src="https://i.imgur.com/VKaxhEL.png" height="100%" width="100%" alt="Disk Sanitization Steps 36"/>
   <br />
   <br />
</p>

Now when our PC is finish restarting, lets log in as Bob on our local user account Desktop2 

36. <p align="center">
   <img src="https://i.imgur.com/zWPkyjq.png" height="100%" width="100%" alt="Disk Sanitization Steps 37"/>
   <br />
   <br />
</p>

Finally, we’ll run some key command-line tools to ensure everything is functioning correctly. Using ping SimoTech.com, we verify network connectivity with the domain controller. The ipconfig command confirms proper network configuration, and net use Bob /domain tests our ability to access domain resources with valid credentials. With these checks, we can confidently confirm that our setup is working as expected.

37. <p align="center">
   <img src="https://i.imgur.com/gmjToYM.png" height="100%" width="100%" alt="Disk Sanitization Steps 38"/>
   <br />
   <br />
</p>

s

38. <p align="center">
   <img src="https://i.imgur.com/VNbd5wb.png" height="100%" width="100%" alt="Disk Sanitization Steps 39"/>
   <br />
   <br />
</p>


Congratulations! We have successfully joined **Desktop2** to the domain as a local user on a Windows 10 PC, configured and analyzed policy settings, and explored **Group Policy Management**.

For administration and troubleshooting, we utilized **Command Line Tools (CMD)** and generated **Resultant Set of Policy (RSOP)** reports to review the implemented policies.

 👉 [Next Lab 6 : Common Active Directory Issues, CMD Commands, PC Offline](https://github.com/melvinensahsl/Common-Active-Directory-Issues-CMD-Commands-PC-Offline)
