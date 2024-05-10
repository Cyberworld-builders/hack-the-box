# Hack the Box
A study of HTB retired box walkthroughs, with an attempt to drill down into the various technologies, tools, and concepts related to each box.

## Retired Box Walkthroughs
- [Rebound](walkthroughs/20240330-Rebound/Rebound.md)

## Topics

I'm going to start dumping these here. As this section grows long, I will identify patterns and try to structure the information in appropriate files and directories with links from here to there.

### NetExec
NetExec is a tool used for executing commands remotely on Windows systems. It allows a user to execute commands or scripts on a remote machine over the network. This tool can be particularly useful for system administrators who need to manage multiple machines in a networked environment.

NetExec typically operates by leveraging the administrative capabilities of Windows systems, such as the ability to execute commands remotely using built-in administrative features like Windows Management Instrumentation (WMI) or remote command execution services like Remote Procedure Call (RPC).

While NetExec itself may not be a specific tool provided by Microsoft or another vendor, there are various implementations and scripts available that offer similar functionality under the name "NetExec" or similar. These implementations may vary in features and compatibility with different versions of Windows.

Overall, NetExec is used to streamline administrative tasks by allowing commands or scripts to be executed on remote Windows systems without needing physical access to those machines.

### SMB (Server Message Block)
It's a network protocol used mainly for providing shared access to files, printers, and serial ports and miscellaneous communications between nodes on a network. It's commonly used in Windows networks.

### Nmap
Nmap, short for Network Mapper, is a widely used open-source tool for network discovery and security auditing. It's designed to help users explore networks, identify what devices are running on those networks (host discovery), discover services and applications running on those devices (service discovery), and gather information about those services (port scanning, version detection, etc.).

### Active Directory

Active Directory (AD) is a directory service developed by Microsoft for use in Windows domain networks. It serves as a central location for network administration and security. Active Directory stores information about objects on a network and makes this information available to users and administrators of the network.

Here are some key components and functions of Active Directory:

1. **Domain Services**: Active Directory Domain Services (AD DS) is the core service within Active Directory. It stores information about users, computers, groups, and other objects on the network. AD DS enables centralized authentication and authorization for network resources.

2. **Domain Controllers**: Domain controllers are servers that run AD DS and manage user authentication and authorization, as well as replication of directory data between domain controllers.

3. **Organizational Units (OUs)**: OUs are containers within Active Directory that are used to organize objects (such as users, computers, and groups) into logical groups. OUs help to simplify administrative tasks by allowing administrators to apply policies and permissions to groups of objects.

4. **Group Policy**: Active Directory Group Policy allows administrators to define and enforce security and configuration settings for users and computers in the domain. Group Policy settings can control everything from user account settings to software installation and system configurations.

5. **LDAP Protocol**: Active Directory supports the Lightweight Directory Access Protocol (LDAP), which is a standard protocol for accessing directory services. LDAP allows applications and services to query and update information stored in Active Directory.

6. **Trust Relationships**: Active Directory supports trust relationships between domains, forests, and external domains. Trust relationships allow users in one domain to access resources in another domain, subject to permissions and security settings.

7. **Security**: Active Directory provides robust security features, including authentication mechanisms (such as Kerberos authentication), access control lists (ACLs) for securing objects, and encryption of network traffic.

Overall, Active Directory plays a central role in managing and securing Windows-based network environments, providing features for user authentication, resource access control, centralized management, and policy enforcement. It is widely used in organizations of all sizes, from small businesses to large enterprises.

### RID Brute Force

RID Brute Force is a technique used in penetration testing and security assessments to enumerate user accounts within a Windows Active Directory domain. It involves attempting to guess or deduce valid Security Identifier (SID) values for user accounts by systematically iterating through possible Relative Identifier (RID) values.

Here's how the RID Brute Force technique typically works:

1. **Understanding SID Structure**: In Windows environments, each user account is assigned a unique Security Identifier (SID). The SID is composed of a domain SID (common to all objects in the domain) and a Relative Identifier (RID) that uniquely identifies each object within the domain.

2. **Enumerating Users**: Attackers may attempt to enumerate existing user accounts by guessing or brute-forcing RID values. Since the domain SID is typically known or can be discovered through reconnaissance, attackers focus on guessing RIDs to identify valid user accounts.

3. **Systematic Enumeration**: Attackers may start with a known RID (such as RID 500 for the built-in Administrator account) and increment or iterate through RID values to identify additional user accounts. They may use automated tools or scripts to perform this enumeration efficiently.

4. **Detecting Valid User Accounts**: For each guessed RID, attackers may attempt to query Active Directory or perform network reconnaissance to determine if a corresponding user account exists. Successful queries indicate the presence of a valid user account with the guessed RID.

5. **Risk and Mitigation**: Brute-forcing RID values can be a time-consuming process, but it can be effective in identifying valid user accounts, especially if weak or predictable RIDs are used. Organizations can mitigate the risk of RID brute force attacks by enforcing strong password policies, monitoring authentication attempts, and implementing security controls to detect and prevent unauthorized access attempts.

It's important to note that while RID Brute Force can be a useful technique for security testing, it should only be performed against systems and networks where authorized, and with appropriate permissions and consent. Unauthorized or malicious use of this technique can violate laws and regulations related to computer security and privacy.


### ASREP Roasting 

ASREP Roasting is a technique used in penetration testing and security assessments to extract password hashes from Active Directory (AD) environments without requiring authentication. It targets user accounts that have the "Do not require Kerberos preauthentication" attribute set in their user account properties.

Here's how ASREP Roasting typically works:

1. **Understanding Kerberos Preauthentication**: In a Windows Active Directory environment, Kerberos is the primary authentication protocol used. When a user attempts to authenticate to a Kerberos-enabled service, they typically present their credentials (username and password) to obtain a Ticket Granting Ticket (TGT). Before issuing the TGT, the Key Distribution Center (KDC) verifies the user's identity by authenticating the user's password using preauthentication.

2. **Do not require Kerberos preauthentication**: Some user accounts in Active Directory may have the "Do not require Kerberos preauthentication" attribute set. This means that the KDC does not require the user to provide preauthentication before issuing a TGT.

3. **ASREP Roasting Attack**: Attackers can target user accounts with the "Do not require Kerberos preauthentication" attribute set. They send authentication requests to the KDC for these accounts, requesting an AS-REP (Authentication Service Reply) ticket without providing preauthentication.

4. **Extracting Password Hashes**: When the KDC receives the authentication request for a user account with the "Do not require Kerberos preauthentication" attribute set, it sends back an AS-REP ticket encrypted with the user's hash (not the session key). Attackers can capture these AS-REP tickets and extract the encrypted password hashes from them.

5. **Cracking Password Hashes**: Once the password hashes are extracted, attackers can attempt to crack them offline using password-cracking tools or techniques. If successful, they can obtain plaintext passwords, which can be used to authenticate as the targeted user account.

6. **Risk and Mitigation**: ASREP Roasting can be an effective technique for obtaining password hashes from Active Directory environments, especially for user accounts with weak or easily guessable passwords. Organizations can mitigate the risk of ASREP Roasting attacks by enforcing strong password policies, regularly auditing Active Directory configurations, and monitoring authentication logs for suspicious activity.

As with any security assessment technique, ASREP Roasting should only be performed against systems and networks where authorized, and with appropriate permissions and consent. Unauthorized or malicious use of this technique can violate laws and regulations related to computer security and privacy.

### Kerberos 

Kerberos is a network authentication protocol that provides strong authentication for client/server applications by using secret-key cryptography. It's widely used in enterprise environments, especially in Windows Active Directory domains, as the primary authentication mechanism.

Here's how Kerberos authentication typically works:

1. **Authentication Process**: When a user wants to access a network service or resource, they authenticate themselves to the Key Distribution Center (KDC) using their credentials (username and password). The KDC is a trusted third-party authentication service that is responsible for issuing tickets for authentication.

2. **Ticket Granting Ticket (TGT)**: Upon successful authentication, the KDC issues the user a Ticket Granting Ticket (TGT), which serves as a proof of the user's identity. The TGT is encrypted with the user's password hash and is valid for a certain period of time.

3. **Service Ticket**: When the user wants to access a specific network service or resource (e.g., a file share or a web server), they present their TGT to the KDC and request a service ticket for that service. The KDC verifies the user's identity and issues a service ticket encrypted with a session key.

4. **Service Authentication**: The user presents the service ticket to the network service or resource they want to access. The service decrypts the ticket using its secret key, verifies the user's identity, and grants access if the user is authorized.

Key features of Kerberos authentication include:

- **Mutual Authentication**: Both the client and the server authenticate each other during the authentication process, providing mutual assurance of each other's identities.

- **Single Sign-On (SSO)**: Once a user obtains a TGT, they can request service tickets for multiple services without needing to re-enter their credentials, providing a seamless authentication experience.

- **Ticket Renewal**: Users can renew their TGTs before they expire, allowing them to maintain continuous access to network resources without needing to re-authenticate.

- **Session Keys**: Kerberos uses session keys for encryption to protect the confidentiality and integrity of communication between the client and the server.

Kerberos is considered a secure authentication protocol, as it uses strong cryptography to protect authentication messages and prevent replay attacks and eavesdropping. It's widely adopted in enterprise environments and is a core component of Microsoft's Active Directory authentication infrastructure.

### Kerberoast attack

A Kerberoast attack is a type of attack targeting the Kerberos authentication protocol used in Windows Active Directory environments. It involves extracting password hashes of user accounts that have Kerberos Service Principal Names (SPNs) associated with them, which are typically service accounts.

Here's how a Kerberoast attack typically works:

1. **Identifying Target Accounts**: Attackers identify user accounts with Kerberos Service Principal Names (SPNs) associated with them. These accounts are typically service accounts used to run various services within the Active Directory environment.

2. **Requesting Ticket-Granting Service (TGS) Tickets**: Attackers request Ticket-Granting Service (TGS) tickets for the identified service accounts from the Key Distribution Center (KDC) using a valid user account or a compromised credential.

3. **Service Ticket Encryption**: The TGS tickets received by the attacker are encrypted with the service account's secret key (derived from its password hash). The attacker captures these TGS tickets.

4. **Extracting Password Hashes**: Attackers extract the encrypted password hashes from the captured TGS tickets. These password hashes are derived from the service account's password.

5. **Offline Password Cracking**: Once the password hashes are extracted, attackers attempt to crack them offline using password-cracking tools or techniques. If successful, they can obtain plaintext passwords, which can be used to authenticate as the service account.

6. **Service Account Compromise**: With the plaintext passwords obtained, attackers can authenticate as the compromised service accounts and potentially gain unauthorized access to sensitive resources or escalate their privileges within the Active Directory environment.

Kerberoast attacks are effective against service accounts because they often have weak or easily guessable passwords, and because they have Kerberos SPNs associated with them, making them easy targets for attackers. Organizations can mitigate the risk of Kerberoast attacks by enforcing strong password policies for service accounts, regularly auditing SPNs, and monitoring for suspicious authentication activity.

As with any security assessment technique, Kerberoast attacks should only be performed against systems and networks where authorized, and with appropriate permissions and consent. Unauthorized or malicious use of this technique can violate laws and regulations related to computer security and privacy.

### Bloodhound

BloodHound is a powerful tool used for Active Directory (AD) reconnaissance and attack surface analysis. It is primarily used in penetration testing, red teaming, and security assessments to identify and visualize attack paths, misconfigurations, and potential security risks within an Active Directory environment.

Here's an overview of the key features and functionalities of BloodHound:

1. **Graph-Based Visualization**: BloodHound generates a graph database that represents the relationships between various objects within an Active Directory environment, such as users, groups, computers, and their associated permissions and trust relationships.

2. **Attack Path Discovery**: BloodHound analyzes the Active Directory environment to identify potential attack paths that an attacker could use to escalate privileges, compromise accounts, or move laterally within the network.

3. **Data Collection**: BloodHound collects and aggregates data from various sources within the Active Directory environment, including user and group memberships, local administrators, group policy settings, trust relationships, and more.

4. **Data Analysis and Querying**: BloodHound provides a query interface that allows users to run custom queries against the graph database to identify specific security issues, such as users with high privileges, over-permissioned accounts, or potential attack paths.

5. **Visual Analytics**: BloodHound includes visual analytics features that allow users to interactively explore the graph database and visually identify security risks, misconfigurations, and potential attack paths.

6. **Automated Reporting**: BloodHound can generate automated reports that summarize the findings of the analysis, including identified security risks, misconfigurations, and recommended remediation steps.

7. **Integration with Other Tools**: BloodHound can be integrated with other security tools and frameworks, such as Metasploit, Empire, and Cobalt Strike, to facilitate exploitation and post-exploitation activities.

BloodHound is widely used by security professionals, including penetration testers, red teamers, and incident responders, to assess and improve the security posture of Active Directory environments. By identifying and visualizing potential attack paths and security risks, BloodHound helps organizations proactively identify and remediate security vulnerabilities before they can be exploited by attackers.

### LDAP

LDAP stands for Lightweight Directory Access Protocol. It is a widely used open standard protocol used to access and maintain directory services over a network. A directory service is a centralized database that stores information about users, groups, computers, and other resources within an organization's network.

Here are some key aspects of LDAP:

1. **Directory Services**: LDAP is commonly used to interact with directory services, such as Microsoft Active Directory, OpenLDAP, and Apache Directory Server. These directory services store structured information in a hierarchical format.

2. **Client-Server Model**: LDAP follows a client-server architecture, where LDAP clients (such as applications or users) communicate with LDAP servers to perform directory operations, such as querying for information, adding or modifying entries, and authenticating users.

3. **Protocol Operations**: LDAP defines a set of protocol operations for interacting with directory services. These operations include:
   - **Bind**: Authentication and establishing a connection with the LDAP server.
   - **Search**: Searching for directory entries based on specified criteria.
   - **Add**: Adding new directory entries.
   - **Modify**: Modifying existing directory entries.
   - **Delete**: Deleting directory entries.
   - **Compare**: Comparing attribute values of directory entries.
   - **Modify DN**: Modifying the distinguished name (DN) of directory entries.

4. **Schema**: LDAP directory services use a schema to define the types of objects and attributes that can be stored in the directory. The schema defines the structure and rules for organizing and accessing directory data.

5. **Security**: LDAP supports various authentication mechanisms, including simple authentication (using a username and password), Kerberos, and Secure Sockets Layer (SSL) encryption. Access control mechanisms can also be configured to restrict access to directory information based on user permissions.

6. **Interoperability**: LDAP is platform-independent and widely supported by various operating systems, directory services, and applications. This makes it a popular choice for integrating different systems and services within an organization's network.

LDAP is commonly used for tasks such as user authentication, authorization, directory lookups, and centralized management of network resources. It plays a crucial role in identity and access management within organizations, facilitating secure and efficient access to directory information across diverse IT environments.

### Password Spray

Password spraying is a cyberattack technique used to gain unauthorized access to user accounts by attempting a few commonly used passwords against many accounts simultaneously. Unlike traditional brute-force attacks, where an attacker tries many passwords against a single user account, password spraying involves trying a few passwords against many accounts.

Here's how password spraying typically works:

1. **Enumeration of User Accounts**: Attackers first identify or enumerate a list of user accounts within the target system or application. This could involve harvesting usernames from public sources, using reconnaissance techniques, or leveraging information leaked from previous data breaches.

2. **Selection of Common Passwords**: Attackers select a small set of commonly used passwords, such as "password," "123456," or variations of common dictionary words. These passwords are chosen because they are likely to be used by a significant number of users and may be successful in gaining unauthorized access.

3. **Attempted Authentication**: Attackers attempt to authenticate to the target system or application using each username/password combination from their list. Rather than trying multiple passwords against a single account (as in a traditional brute-force attack), attackers try a few passwords against many accounts (hence the term "password spraying").

4. **Detection Avoidance**: To avoid detection and account lockouts, attackers may use techniques such as slow and randomized authentication attempts, spreading out attempts over time, or targeting multiple systems or applications simultaneously.

5. **Successful Access**: If the attackers are successful in guessing a valid username/password combination, they gain unauthorized access to the target account. They may then use this access to further their objectives, such as accessing sensitive data, escalating privileges, or launching additional attacks within the network.

Password spraying attacks are effective against systems with weak password policies, where users commonly choose easily guessable passwords. To mitigate the risk of password spraying attacks, organizations should enforce strong password policies, implement multi-factor authentication (MFA), monitor for unusual authentication activity, and educate users about the importance of using strong, unique passwords.

### BloodyAD

"BloodyAD" appears to be a term or a tool without a widely recognized context. Without further information, it's challenging to provide a precise explanation of what "BloodyAD" refers to. It could be:

1. **Security Tool or Framework**: "BloodyAD" might be the name of a security tool, framework, or script designed for Active Directory (AD) security testing, penetration testing, or red teaming purposes. Tools with names like this are often developed by cybersecurity professionals or organizations to automate security assessments and exploit Active Directory vulnerabilities.

2. **Custom Tool or Script**: Alternatively, "BloodyAD" could be a custom tool or script developed by an individual or organization for specific security testing or research purposes. It might have features tailored to assess AD security, detect misconfigurations, exploit vulnerabilities, or perform other actions related to AD security testing.

3. **Placeholder or Example**: In some cases, "BloodyAD" could be used as a placeholder or an example name without specific meaning. It might be used in discussions, presentations, or documentation as a hypothetical tool or scenario.

To provide a more accurate explanation, additional context or clarification about the specific context in which "BloodyAD" is being used would be helpful. This could include details about where you encountered the term, its usage, or any related information that could shed light on its meaning.