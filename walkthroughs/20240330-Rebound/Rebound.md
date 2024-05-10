# Rebound

## Info
- ***Hacker:*** IppSec
- ***Operating System:*** Windows

## Notes
- Lots of Active Directory
- 

# Table of Contents

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
