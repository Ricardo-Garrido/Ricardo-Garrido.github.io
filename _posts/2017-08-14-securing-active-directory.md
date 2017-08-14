---
layout: post
show-avatar: true
comments: true
social-share: true
title: Securing Active Directory
date: 2017-08-14 00:00:00 +0000
image: ''
tags:
- Active Directory
subtitle: ''
bigimg: ''
---


Active Directory has several levels of administration beyond the Domain Admins group. In a previous post, I explored: “Securing Domain Controllers to Improve Active Directory Security” which explores ways to better secure Domain Controllers and by extension, Active Directory. For more information on Active Directory specific rights and permission review my post “Scanning for Active Directory Privileges & Privileged Accounts.”

This post provides information on how Active Directory is typically administered and the associated roles & rights.

* Domain Admins is the AD group that most people think of when discussing Active Directory administration. This group has full admin rights by default on all domain-joined servers and workstations, Domain Controllers, and Active Directory. It gains admin rights on domain-joined computers since when these systems are joined to AD, the Domain Admins group is added to the computer’s Administrators group.
* Enterprise Admins is a group in the forest root domain that has full AD rights to every domain in the AD forest. It is granted this right through membership in the Administrators group in every domain in the forest.
* Administrators in the AD domain, is the group that has default admin rights to Active Directory and Domain Controllers and provides these rights to Domain Admins and Enterprise Admins, as well as any other members.
* Schema Admins is a group in the forest root domain that has the ability to modify the Active Directory forest schema.

Since the Administrators group is the domain group that provides full rights to AD and Domain Controllers, it’s important to monitor this group’s membership (including all nested groups). The Active Directory PowerShell cmdlet “Get-ADGroupMember” can provide group membership information.

Default groups in Active Directory often have extensive rights – many more than typically required. For this reason, we don’t recommend using these groups for delegation. Where possible, perform custom delegation to ensure the principle of least privilege is followed. The following groups should have a “DC” prefix added to them since the scope applies to Domain Controllers by default. Furthermore, they have elevated rights on Domain Controllers and should be considered effectively Domain Controller admins.

* Backup Operators is granted the ability to logon to, shut down, and perform backup/restore operations on Domain Controllers (assigned via the Default Domain Controllers Policy GPO). This group cannot directly modify AD admin groups, though associated privileges provides a path for escalation to AD admin. Backup Operators have the ability to schedule tasks which may provide an escalation path. They also are able to clear the event logs on Domain Controllers.
* Print Operators is granted the ability to manage printers and load/unload device drivers on Domain Controllers as well as manage printer objects in Active Directory. By default, this group can logon to Domain Controllers and shut them down. This group cannot directly modify AD admin groups.
* Server Operators is granted the ability to logon to, shut down, and perform backup/restore operations on Domain Controllers (assigned via the Default Domain Controllers Policy GPO). This group cannot directly modify AD admin groups, though associated privileges provides a path for escalation to AD admin.

To a lesser extend, we’ll group Remote Desktop Users into this category as well.

* Remote Desktop Users is a domain group designed to easily provide remote access to systems. In many AD domains, this group is added to the “Allow log on through Terminal Services” right in the Default Domain Controllers Policy GPO providing potential remote logon capability to DCs.

We also see that many times the following is configured via GPO linked to the Domain Controllers OU:

* Remote Desktop Users: granted “Allow log on through Terminal Services” right via Group Policy linked to the Domain Controllers OU.
* Server Operators: granted “Allow log on through Terminal Services” right via Group Policy linked to the Domain Controllers OU.
* Server Operators: granted “Log on as a batch job” right via GPO providing the ability to schedule tasks.