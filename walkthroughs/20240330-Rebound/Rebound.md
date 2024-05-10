# Rebound

## Info
- ***Hacker:*** IppSec
- ***Operating System:*** Windows

## Details
- All pure Active Directory.
- No web exploitation.
- No ADCS attacks either.

## Summary
1. RID brute force to get an AS reproast-able account, however, you can't crack the hash.
2. Instead, you use this config to perform a Cerberos attack, which normally requires credentials. This gets you the hash to a different service which you can crack.
3. From there, we find a well-hidden bloudhound path to reset an account to get onto the system where there's another user logged into the box.
4. Use a cross-session relay attack with remote potato to get their NTLM hash which leads us to be able to read AGMSA password that a group-managed service account but it has constrained delegation.
5. Because of the constrained delegation, we can't create a forwardable-ticket with the group-managed service account. However, we can abuse role-based constrained delegation to create a forwardable service ticket and then create a TGS with the two tickets that enables us to impersonate the DC and do a DC-sync.


## Tools, Tech, and Concepts

### Nmap
Nmap, short for Network Mapper, is a widely used open-source tool for network discovery and security auditing. It's designed to help users explore networks, identify what devices are running on those networks (host discovery), discover services and applications running on those devices (service discovery), and gather information about those services (port scanning, version detection, etc.).

### NetExec
NetExec is a tool used for executing commands remotely on Windows systems. It allows a user to execute commands or scripts on a remote machine over the network. This tool can be particularly useful for system administrators who need to manage multiple machines in a networked environment.

NetExec typically operates by leveraging the administrative capabilities of Windows systems, such as the ability to execute commands remotely using built-in administrative features like Windows Management Instrumentation (WMI) or remote command execution services like Remote Procedure Call (RPC).

While NetExec itself may not be a specific tool provided by Microsoft or another vendor, there are various implementations and scripts available that offer similar functionality under the name "NetExec" or similar. These implementations may vary in features and compatibility with different versions of Windows.

Overall, NetExec is used to streamline administrative tasks by allowing commands or scripts to be executed on remote Windows systems without needing physical access to those machines.

### SMB (Server Message Block)
It's a network protocol used mainly for providing shared access to files, printers, and serial ports and miscellaneous communications between nodes on a network. It's commonly used in Windows networks.

## Table of Contents

- [00:00 - Introduction](#introduction)
- [01:07 - Start of nmap then checking SMB Shares](#start-of-nmap-then-checking-smb-shares)
- [04:05 - Using NetExec to do a RID Brute Force and increase the maximum to 10000](#using-netexec-to-do-a-rid-brute-force-and-increase-the-maximum-to-10000)
- [07:00 - Using vim g!/{string}/d to delete all lines that do not contain something to build wordlists](#using-vim-gstringd-to-delete-all-lines-that-do-not-contain-something-to-build-wordlists)
- [10:40 - Using ASREP Roasting to perform a Kerberoast attack without authentication](#using-asrep-roasting-to-perform-a-kerberoast-attack-without-authentication)
- [17:40 - Using NetExec to run Bloodhound as ldap_monitor](#using-netexec-to-run-bloodhound-as-ldap_monitor)
- [25:30 - Discovering Oorend can add themself to the ServiceMgmt group, which can take over the WinRM Account](#discovering-oorend-can-add-themself-to-the-servicemgmt-group-which-can-take-over-the-winrm-account)
- [28:30 - Spraying the password of the ldap_monitor account to discover it shares oorend's password](#spraying-the-password-of-the-ldap_monitor-account-to-discover-it-shares-oorends-password)
- [34:20 - Showing BloodyAD to add Oorend to ServiceMGMT then abusing the GenericAll to Service Users, so we can reset WinRM_SVC's password](#showing-bloodyad-to-add-oorend-to-servicemgmt-then-abusing-the-genericall-to-service-users-so-we-can-reset-winrms-password)
- [40:30 - Showing a more opsec friendly way to take over WINRM_SVC by abusing shadow credentials so we don't change the accounts password](#showing-a-more-opsec-friendly-way-to-take-over-winrmsvc-by-abusing-shadow-credentials-so-we-dont-change-the-accounts-password)
- [50:00 - As WINRM_SVC we cannot run some commands like qwinsta or tasklist. Using RunasCS, to switch our login to a non-remote login which will let us run these commands](#as-winrmsvc-we-cannot-run-some-commands-like-qwinsta-or-tasklist-using-runascs-to-switch-our-login-to-a-non-remote-login-which-will-let-us-run-these-commands)
- [54:10 - Performing a Cross Session attack with Remote Potato to steal the NTLMv2 Hash of another user logged into the same box we are](#performing-a-cross-session-attack-with-remote-potato-to-steal-the-ntlmv2-hash-of-another-user-logged-into-the-same-box-we-are)
- [59:20 - Using NetExec to read GMSA Passwords as TBRADY](#using-netexec-to-read-gmsa-passwords-as-tbrady)
- [1:04:00 - Using findDelagation.py to show constrained delegation from the GMSA delegator account](#using-finddelagationpy-to-show-constrained-delegation-from-the-gmsa-delegator-account)
- [1:11:20 - Using RBCD.py to setup the Resource-Based Constrained Delegation so we can get a forwardable ticket to abuse delegators delegation and impersonate users](#using-rbcdpy-to-setup-the-resource-based-constrained-delegation-so-we-can-get-a-forwardable-ticket-to-abuse-delegators-delegation-and-impersonate-users)
- [1:18:20 - Using our ticket that impersonates DC01 and performing secretsdump and then getting Administrator's hash so we can login](#using-our-ticket-that-impersonates-dc01-and-performing-secretsdump-and-then-getting-administrators-hash-so-we-can-login)

## Introduction
[00:00 - Introduction](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=0s)

## Start of nmap then checking SMB Shares
[01:07 - Start of nmap then checking SMB Shares](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=67s)

### Enumerate services
```bash
sudo nmap -sC -sV -oA nmap/rebound 10.10.11.231
```

### Results
```bash
less nmap/rebound



```

### Add Domains to Hosts file and Scan again
```bash
vi nmap/rebound

10.10.11.231    rebound.htb dc01.rebound.htb

sudo nmap -sC -sV -oA nmap/rebound 10.10.11.231
```


```bash
netexec smb 10.10.11.31
```

## Using NetExec to do a RID Brute Force and increase the maximum to 10000
[04:05 - Using NetExec to do a RID Brute Force and increase the maximum to 10000](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=245s)

## Using vim g!/{string}/d to delete all lines that do not contain something to build wordlists
[07:00 - Using vim g!/{string}/d to delete all lines that do not contain something to build wordlists](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=420s)

## Using ASREP Roasting to perform a Kerberoast attack without authentication
[10:40 - Using ASREP Roasting to perform a Kerberoast attack without authentication](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=640s)

## Using NetExec to run Bloodhound as ldap_monitor
[17:40 - Using NetExec to run Bloodhound as ldap_monitor](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=1060s)

## Discovering Oorend can add themself to the ServiceMgmt group, which can take over the WinRM Account
[25:30 - Discovering Oorend can add themself to the ServiceMgmt group, which can take over the WinRM Account](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=1530s)

## Spraying the password of the ldap_monitor account to discover it shares oorend's password
[28:30 - Spraying the password of the ldap_monitor account to discover it shares oorend's password](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=1710s)

## Showing BloodyAD to add Oorend to ServiceMGMT then abusing the GenericAll to Service Users, so we can reset WinRM_SVC's password
[34:20 - Showing BloodyAD to add Oorend to ServiceMGMT then abusing the GenericAll to Service Users, so we can reset WinRM_SVC's password](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=2060s)

## Showing a more opsec friendly way to take over WINRM_SVC by abusing shadow credentials so we don't change the accounts password
[40:30 - Showing a more opsec friendly way to take over WINRM_SVC by abusing shadow credentials so we don't change the accounts password](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=2430s)

## As WINRM_SVC we cannot run some commands like qwinsta or tasklist. Using RunasCS, to switch our login to a non-remote login which

 will let us run these commands
[50:00 - As WINRM_SVC we cannot run some commands like qwinsta or tasklist. Using RunasCS, to switch our login to a non-remote login which will let us run these commands](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=3000s)

## Performing a Cross Session attack with Remote Potato to steal the NTLMv2 Hash of another user logged into the same box we are
[54:10 - Performing a Cross Session attack with Remote Potato to steal the NTLMv2 Hash of another user logged into the same box we are](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=3250s)

## Using NetExec to read GMSA Passwords as TBRADY
[59:20 - Using NetExec to read GMSA Passwords as TBRADY](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=3560s)

## Using findDelagation.py to show constrained delegation from the GMSA delegator account
[1:04:00 - Using findDelagation.py to show constrained delegation from the GMSA delegator account](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=3840s)

## Using RBCD.py to setup the Resource-Based Constrained Delegation so we can get a forwardable ticket to abuse delegators delegation and impersonate users
[1:11:20 - Using RBCD.py to setup the Resource-Based Constrained Delegation so we can get a forwardable ticket to abuse delegators delegation and impersonate users](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=4280s)

## Using our ticket that impersonates DC01 and performing secretsdump and then getting Administrator's hash so we can login
[1:18:20 - Using our ticket that impersonates DC01 and performing secretsdump and then getting Administrator's hash so we can login](https://www.youtube.com/watch?v=oUIoH4yBT3k&t=4700s)
