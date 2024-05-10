# Rebound

## Info
- ***Hacker:*** IppSec
- ***Operating System:*** Windows

## Notes
- Lots of Active Directory
- 

# Table of Contents

- [00:00 - Introduction](#00:00---introduction)
- [01:07 - Start of nmap then checking SMB Shares](#01:07---start-of-nmap-then-checking-smb-shares)
- [04:05 - Using NetExec to do a RID Brute Force and increase the maximum to 10000](#04:05---using-netexec-to-do-a-rid-brute-force-and-increase-the-maximum-to-10000)
- [07:00 - Using vim g!/{string}/d to delete all lines that do not contain something to build wordlists](#07:00---using-vim-gstringd-to-delete-all-lines-that-do-not-contain-something-to-build-wordlists)
- [10:40 - Using ASREP Roasting to perform a Kerberoast attack without authentication](#10:40---using-asrep-roasting-to-perform-a-kerberoast-attack-without-authentication)
- [17:40 - Using NetExec to run Bloodhound as ldap_monitor](#17:40---using-netexec-to-run-bloodhound-as-ldap_monitor)
- [25:30 - Discovering Oorend can add themself to the ServiceMgmt group, which can take over the WinRM Account](#25:30---discovering-oorend-can-add-themself-to-the-servicemgmt-group-which-can-take-over-the-winrm-account)
- [28:30 - Spraying the password of the ldap_monitor account to discover it shares oorend's password](#28:30---spraying-the-password-of-the-ldap_monitor-account-to-discover-it-shares-oorends-password)
- [34:20 - Showing BloodyAD to add Oorend to ServiceMGMT then abusing the GenericAll to Service Users, so we can reset WinRM_SVC's password](#34:20---showing-bloodyad-to-add-oorend-to-servicemgmt-then-abusing-the-genericall-to-service-users-so-we-can-reset-winrms-password)
- [40:30 - Showing a more opsec friendly way to take over WINRM_SVC by abusing shadow credentials so we don't change the accounts password](#40:30---showing-a-more-opsec-friendly-way-to-take-over-winrmsvc-by-abusing-shadow-credentials-so-we-dont-change-the-accounts-password)
- [50:00 - As WINRM_SVC we cannot run some commands like qwinsta or tasklist. Using RunasCS, to switch our login to a non-remote login which will let us run these commands](#50:00---as-winrmsvc-we-cannot-run-some-commands-like-qwinsta-or-tasklist-using-runascs-to-switch-our-login-to-a-non-remote-login-which-will-let-us-run-these-commands)
- [54:10 - Performing a Cross Session attack with Remote Potato to steal the NTLMv2 Hash of another user logged into the same box we are](#54:10---performing-a-cross-session-attack-with-remote-potato-to-steal-the-ntlmv2-hash-of-another-user-logged-into-the-same-box-we-are)
- [59:20 - Using NetExec to read GMSA Passwords as TBRADY](#59:20---using-netexec-to-read-gmsa-passwords-as-tbrady)
- [1:04:00 - Using findDelagation.py to show constrained delegation from the GMSA delegator account](#1:04:00---using-finddelagationpy-to-show-constrained-delegation-from-the-gmsa-delegator-account)
- [1:11:20 - Using RBCD.py to setup the Resource-Based Constrained Delegation so we can get a forwardable ticket to abuse delegators delegation and impersonate users](#1:11:20---using-rbcdpy-to-setup-the-resource-based-constrained-delegation-so-we-can-get-a-forwardable-ticket-to-abuse-delegators-delegation-and-impersonate-users)
- [1:18:20 - Using our ticket that impersonates DC01 and performing secretsdump and then getting Administrator's hash so we can login](#1:18:20---using-our-ticket-that-impersonates-dc01-and-performing-secretsdump-and-then-getting-administrators-hash-so-we-can-login)

## 00:00 - Introduction
[Jump to Introduction](#00:00---introduction)

## 01:07 - Start of nmap then checking SMB Shares
[Jump to Start of nmap then checking SMB Shares](#01:07---start-of-nmap-then-checking-smb-shares)

## 04:05 - Using NetExec to do a RID Brute Force and increase the maximum to 10000
[Jump to Using NetExec to do a RID Brute Force and increase the maximum to 10000](#04:05---using-netexec-to-do-a-rid-brute-force-and-increase-the-maximum-to-10000)

## 07:00 - Using vim g!/{string}/d to delete all lines that do not contain something to build wordlists
[Jump to Using vim g!/{string}/d to delete all lines that do not contain something to build wordlists](#07:00---using-vim-gstringd-to-delete-all-lines-that-do-not-contain-something-to-build-wordlists)

## 10:40 - Using ASREP Roasting to perform a Kerberoast attack without authentication
[Jump to Using ASREP Roasting to perform a Kerberoast attack without authentication](#10:40---using-asrep-roasting-to-perform-a-kerberoast-attack-without-authentication)

## 17:40 - Using NetExec to run Bloodhound as ldap_monitor
[Jump to Using NetExec to run Bloodhound as ldap_monitor](#17:40---using-netexec-to-run-bloodhound-as-ldap_monitor)

## 25:30 - Discovering Oorend can add themself to the ServiceMgmt group, which can take over the WinRM Account
[Jump to Discovering Oorend can add themself to the ServiceMgmt group, which can take over the WinRM Account](#25:30---discovering-oorend-can-add-themself-to-the-servicemgmt-group-which-can-take-over-the-winrm-account)

## 28:30 - Spraying the password of the ldap_monitor account to discover it shares oorend's password
[Jump to Spraying the password of the ldap_monitor account to discover it shares oorend's password](#28:30---spraying-the-password-of-the-ldap_monitor-account-to-discover-it-shares-oorends-password)

## 34:20 - Showing BloodyAD to add Oorend to ServiceMGMT then abusing the GenericAll to Service Users, so we can reset WinRM_SVC's password
[Jump to Showing BloodyAD to add Oorend to ServiceMGMT then abusing the GenericAll to Service Users, so we can reset WinRM_SVC's password](#34:20---showing-bloodyad-to-add-oorend-to-servicemgmt-then-abusing-the-genericall-to-service

-users-so-we-can-reset-winrms-password)

## 40:30 - Showing a more opsec friendly way to take over WINRM_SVC by abusing shadow credentials so we don't change the accounts password
[Jump to Showing a more opsec friendly way to take over WINRM_SVC by abusing shadow credentials so we don't change the accounts password](#40:30---showing-a-more-opsec-friendly-way-to-take-over-winrmsvc-by-abusing-shadow-credentials-so-we-dont-change-the-accounts-password)

## 50:00 - As WINRM_SVC we cannot run some commands like qwinsta or tasklist. Using RunasCS, to switch our login to a non-remote login which will let us run these commands
[Jump to As WINRM_SVC we cannot run some commands like qwinsta or tasklist. Using RunasCS, to switch our login to a non-remote login which will let us run these commands](#50:00---as-winrmsvc-we-cannot-run-some-commands-like-qwinsta-or-tasklist-using-runascs-to-switch-our-login-to-a-non-remote-login-which-will-let-us-run-these-commands)

## 54:10 - Performing a Cross Session attack with Remote Potato to steal the NTLMv2 Hash of another user logged into the same box we are
[Jump to Performing a Cross Session attack with Remote Potato to steal the NTLMv2 Hash of another user logged into the same box we are](#54:10---performing-a-cross-session-attack-with-remote-potato-to-steal-the-ntlmv2-hash-of-another-user-logged-into-the-same-box-we-are)

## 59:20 - Using NetExec to read GMSA Passwords as TBRADY
[Jump to Using NetExec to read GMSA Passwords as TBRADY](#59:20---using-netexec-to-read-gmsa-passwords-as-tbrady)

## 1:04:00 - Using findDelagation.py to show constrained delegation from the GMSA delegator account
[Jump to Using findDelagation.py to show constrained delegation from the GMSA delegator account](#1:04:00---using-finddelagationpy-to-show-constrained-delegation-from-the-gmsa-delegator-account)

## 1:11:20 - Using RBCD.py to setup the Resource-Based Constrained Delegation so we can get a forwardable ticket to abuse delegators delegation and impersonate users
[Jump to Using RBCD.py to setup the Resource-Based Constrained Delegation so we can get a forwardable ticket to abuse delegators delegation and impersonate users](#1:11:20---using-rbcdpy-to-setup-the-resource-based-constrained-delegation-so-we-can-get-a-forwardable-ticket-to-abuse-delegators-delegation-and-impersonate-users)

## 1:18:20 - Using our ticket that impersonates DC01 and performing secretsdump and then getting Administrator's hash so we can login
[Jump to Using our ticket that impersonates DC01 and performing secretsdump and then getting Administrator's hash so we can login](#1:18:20---using-our-ticket-that-impersonates-dc01-and-performing-secretsdump-and-then-getting-administrators-hash-so-we-can-login)
