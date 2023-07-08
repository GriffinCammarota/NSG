<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create sample files with different permissions
- Attempt to access those files as a normal user
- Create an New Security Group, assign permissions and test access

<h2>Actions and Observations</h2>


<p>
  Create Some sample file shares with different permissions:
<ol>
  <li>Connect and log in to DC-1 as your domain admin (mydomain.com\bart_admin)</li>
  <li>Connect to Client-1 as a normal user (mydomain.com\generateduser)</li>
  <li>On DC-1, on the C:\ drive create 4 folders: "read-access", "write access", "no-access", and "accounting"</li>
  <li>Go to the "read-access" folder's properties and go to share. Type "Domain Users" and set the permissions to "read" </li>
  <li>Now on the "write-access" give Domain Users Read/Write access</li>
  <li>On the "no-access" folder only give admins permissions to "read/write"</li>
  <li>Skip permissions for "accounting" for now</li>
</ol>
</p>
<br />

<p>
  <ul>
    <li>On client-1, go to the shared folder (start, run, \\dc-1)</li>
    <li>Try to access the folders we just created. We should only be able to read in the read only folder, read and create files in the "write-access" folder and we shouldn't be able to open the "no access" folder</li>
  </ul>
</p>
<br />


<p>
  Create an Accountants Security Group, assign permissions, and set access
<ol>
  <li>Back on DC-1 in Active Directory, create a security group called "Accountants"</li>
  <li>On the "accounting" folder we created earlier set the permissions to:</li>
  <li>Folder: "Accounting", Security Group: ACCOUNTANTS, Permissions: "Read/Write"</li>
  <li>Back on client-1 if we try to access the "accounting" folder it should fail</li>
  <li>Now lets log out of client-1 with that user</li>
  <li>On DC-1 make a user a member of the accountants security group. In Active Directory Users and Computers, go to "_SECURITY_GROUPS" and open the "ACCOUNTANTS" Security Group </li>
  <li>Add the user you were signed into before to the "members" tab</li>
  <li>Sign back in to client-1 </li>
  <li>We now should be able to access the accounting folder (side note: we signed out of client-1 earlier because permissions update when you log into the client. So we'd only be able to access the accounting file if we had logged out then back in like we did)</li>
</ol>
</p>
<br />
