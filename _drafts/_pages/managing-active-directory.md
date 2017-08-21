---
layout: post
show-avatar: true
comments: true
social-share: true
title: Managing Active Directory
date: 2017-08-21 00:00:00 +0000
image: ''
tags: []
subtitle: ''
bigimg: ''
---


Move inactive computer accounts to a specific OU

(Search-ADAccount -AccountInactive -TimeSpan 90.00:00:00 | Where-Object -FilterScript {   $_.ObjectClass -eq 'co<span style="font-size: 1rem;">mputer' }).DistinguishedName | Move-ADObject -TargetPath "OU=Computer,OU=zDISABLED,DC=domain,DC=local"</span>

Move inactive user accounts to a specific OU

(Search-ADAccount -AccountInactive -TimeSpan 90.00:00:00 | Where-Object -FilterScript { $_.ObjectClass -eq 'user' }).DistinguishedName | Move-ADObject -TargetPath "OU=User,OU=zDISABLED,DC=domain,DC=local"