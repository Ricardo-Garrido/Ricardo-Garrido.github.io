---
layout: page
show-avatar: true
title: System Center Operations Manager
date: 2018-04-04 13:57:34 +0000
---
\#Agents - Recalculate Monitoring State

Get-SCOMAgent | foreach { $_.HostComputer.RecalculateMonitoringState() }  

\#Agents - Reset Monitoring State

Get-SCOMAgent | foreach { $_.HostComputer.ResetMonitoringState() }  