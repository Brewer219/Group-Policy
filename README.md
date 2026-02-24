<p align="center">
<img width="600" alt="Microsoft Active Directory Logo" src="images/active-directory-seeklogo.png" />

</p>

<h1>Group Policy and Account Management</h1>
This project continues from the previously established Active Directory Domain Services environment hosted in Microsoft Azure. The lab demonstrates how Group Policy can be used to enforce account lockout thresholds, how administrators unlock and reset accounts, and how enabling or disabling accounts impacts authentication. Additionally, system and security logs are reviewed to understand how these events are recorded on both the Domain Controller and client machines. As an additional observation, external failed login attempts were visible in Event Viewer due to Remote Desktop exposure, providing practical insight into real-world security considerations such as firewall rules and account lockout policies.

This exercise reinforces real-world administrative tasks commonly performed by IT support and system administrators in enterprise environments.<br />



<h2>Environments and Technologies Used</h2>

<img src="https://skillicons.dev/icons?i=azure,windows,powershell" />

- Platform: Microsoft Azure (Virtual Machines/Compute)
- Remote Access: Microsoft Remote Desktop 3389
- Services: Active Directory Domain Services
- Tools: PowerShell, Group Policy Management, Windows Event Viewer
- Operating Systems: Windows 10/11



<h3>High-Level Deployment and Configuration Steps</h3>

<img src="images/DC and AD diagram.gif" width="400" alt="DC and AD Diagram">

**Key Actions**
1. Configure account lockout policy in Group Policy Management.
2. Trigger account lockout through repeated failed login attempts.
3. Unlock and reset a user account in Active Directory Users and Computers.
4. Disable and re-enable user accounts to observe authentication behavior.
5. Review security and authentication logs on both domain controller and client machines.
6. Optional: Observe external failed login attempts due to Remote Desktop exposure.
   
> [!IMPORTANT]
> Each step includes written instructions followed by a corresponding screenshot.
<br>Expand the **See screenshots** section to view the images.


> [!NOTE] 
> This project builds upon a prior lab where the Azure environment, virtual machines, Active Directory Domain Services, and DNS configuration were initially deployed. <br /> **[Part 2. Active Directory User & Client Management](https://github.com/MikeSays-1/Active-Directory-User-Client-Management)**



<h4>CONFIGURE ACCOUNT LOCKOUT POLICY </h4>
<p>Under the default configuration, users can enter an incorrect password an unlimited number of times. In this step, a lockout policy will be configured to restrict failed login attempts. Access the Group Policy Management settings on the domain controller by searching, "Group Policy Managment".  Expand forest > domain > mydomain.com, and right-click Default Domain Policy and select "Edit". </p>

- Objective: Restrict failed login attempts to enhance security.
- Instructions:
  1. Access: "Group Policy Management" on the domain controller by searching for it.
  2. Expand: Forest > Domains >
     mydomain.com.
  3. Right-click "Default Domain Policy" and select "Edit".
  4. Navigate to "Computer Configuration >
   Policies > Windows Settings > Security Settings >
   Account Policies.
 5. Define the "Account lockout Threshold"  
  policy to enforce automatic lockouts after "5
failed login attempts"
 6. Ensure the "Account lockout duration" and
 Reset account lockout counter after" settings
are configured as desired.

<img width="318" height="299" alt="Step 5 Group Policy Account Lockout Policy duration" src="https://github.com/user-attachments/assets/1d213a5e-8e74-4159-8a1f-e749687753e3" />


> [!NOTE] 
> After applying settings. Observe the "Account lockout duration" and "Reset Account lockout counter after" settings change.


<h5> SIMULATE FAILED LOGINS AND LOCKOUT </h5>
<p>Select any username from our _EMPLOYEES OU and Remote Desktop(RDP) into "vm-client-1",intentionally enter incorrect credentials multiple times to trigger the configured lockout threshold. On "vm-dc-1", confirm the account status within Active Directory. Right-click the affected user and select "Properties", review the Account tab. Observe the checkbox and Account status "This account is currently locked out on this Active Directory Domain Controller".</p>
- Objective: Trigger the account lockout
policy to observe it effects.
- Intructions:
 1. Select a username from the **_EMPLOYEES OU** and RDP into **vm-client-1**.
 2. Intentionally enter incorrect credentials
 multiple times to trigger the lockout.
 3. On **vm-dc-1**, confirm the account status in
 Active Directory.
 4. Right-click the affected user, select
 **Properties**, and review the **Account** tab to 
 see the lockout status.
 <details><summary>See screenshots</summary>
<img src="images/Step 3a.PNG" width="60%" >
</details> 
 
<img width="413" height="343" alt="CyberLab password lockout" src="https://github.com/user-attachments/assets/ea0708c9-5a88-43e3-b53c-7aceedeb2455" />


<h3>ACCOUNT RECOVERY ACTIONS</h3>
- Objective: Unlock and reset user accounts
in Active Directory.
- Instructions:
1. Mark the checkbox to unlock the affected
account and apply the changes.
2. Optionally, choose **Reset Password** to 
rest the password without making changes.
3. Verify the unlock by attempting to log in with
the test user's credentials.


<details><summary>See screenshots</summary>
<img src="images/Step 3a.PNG" width="60%" >
</details> 

Verify the unlock by attempting to login using test user's credentials.</p>

<details><summary>See screenshots</summary>
<img src="images/Step 3b.PNG" width="60%" >
</details> 

> [!NOTE] 
> Opened PowerShell and we can see the test user is logged in. 

<h4>DISABLE AND Enable USER ACCOUNTS</h4>
<p>Temporarily disable the same user account in Active Directory, attempt authentication to observe the system response, then re-enable the account and confirm restored access.</p>
- Objective: Observe the effects of disabling
and re-enabling accounts on authentication.
- Instructions:
1. Temporarily disable the user account in Active
Directory.
2. Attempt authentication to see the system response.
3. Re-enable the account and confirm restored access.

<details><summary>See screenshots</summary>
<img src="images/Step 3b.PNG" width="60%" >
</details> 

<img width="393" height="377" alt="Step 10 Group Policy disabling meg dev user account by right clicking on user" src="https://github.com/user-attachments/assets/6e373ef8-cd67-4327-b0eb-40cc323d2d25" />

<img width="382" height="453" alt="Step 12 Group Policy and Managing meg dev enabled account" src="https://github.com/user-attachments/assets/44cd2376-bfa9-4907-927e-de64dda12390" />

 

<h5>LOG OBSERVATION AND ANALYSIS</h5>
<p>Review authentication and security logs on the domain controller and client machine using Event Viewer to identify lockout events, failed login attempts, and account status changes. Open Event Viewer on "vm-dc-1". Expand "Windows Log", select "Security" to view the logs. Right-click "Security" and search for consecutive Audit Failures.</p>
- Objective: Review authentication and 
security logs to understand account status
changes.
- Instructions:
1. Open **Event Viewer** on **vm-dc-1** and
expand **Windows Logs**.
2. Select **Security** to view logs and search
for consecutive **Audit Failures**.
3. Identify failed logins categorized under
**Credential Validation** with **Audit Failure** as
the keyword.
4. Open Event Viewer on the client machine and 
observe the failed login attempts.
<details><summary>See screenshot</summary>
<img src="images/Step 3b.PNG" width="60%" >
</details> 

<img width="382" height="453" alt="Step 12 Group Policy and Managing meg dev enabled account" src="https://github.com/user-attachments/assets/ee724b65-94cd-4492-8d31-d9307f09cdea" />



Failed logins, fall under "Credential Validation" in the Task Category with "Audit Failure" as the Keyword. Successful logins will have "Audit Success" as the Keyword.
<details><summary>See screenshot</summary>
<img src="images/Step 3b.PNG" width="60%" >
</details> 

<img width="360" height="335" alt="Group Policy explore event view logs redo" src="https://github.com/user-attachments/assets/64ae4631-c997-47b7-bee1-7466fb1293a0" />


Open Event Viewer on our Client. As a test user, you will be denied access. Run Event Viewer as Admin, and use Jane Doe's credentials. Look for the consecutive Audit Failures.

<details><summary>See screenshots</summary>
<img src="images/Step 3b.PNG" width="60%" >
</details> 

<img width="357" height="339" alt="Group Policy logged in as jane admin in client vm to see meg dev audit failures redo" src="https://github.com/user-attachments/assets/24e64027-4ab2-4e22-bcf4-ee3dc5ff5765" />

<img width="350" height="337" alt="Group Policy jane admin logged in to event viewer see megdev audit failure from the IP address" src="https://github.com/user-attachments/assets/a9de59ec-b3c3-4fa8-9330-2e8f5b7133ee" />




<h6>Bonus - Observing External Authentication Attempts (Bots)</h6>
-Objective: Analyze Failed login attempts due to external exposure.
-Instructions:
1. Review Event Viewer logs for a high number of
consecutive **Audit Failure** logs.
2. Note the these logs result from automated
bots scanning for vulnerable systems due to RDP
exposure.
3. Highlight the importance of strong
authentication practices and firewall
configurations.


<details><summary>See screenshots</summary>
<img src="images/Step 5c.PNG" width="60%" >
</details>

> [!NOTE]
> From the Domain Controller VM, the username “Sales” appears because the Domain Controller is responsible for credential validation and logs the actual username submitted during each authentication attempt.

<details><summary>See screenshots</summary>
<img src="images/Step 5d.PNG" width="60%" >
</details> 

> [!NOTE]
> From the Client 1 VM, no username because the attempt was rejected early in authetication handshake.

> [!NOTE]
> The Windows Firewall was temporarily disabled in this lab to simplify connectivity and reduce rule conflicts. This is not required for Active Directory and would not be recommended in a production environment.

