---
title: SQL Server Configuration
date: 2018-01-16 10:54:18 +0000
subtitle: Quick configuration best practices
layout: ''
tags: []
---
As a system admin I often find myself working very closely with SQL server. Here is a collection of resources that I find useful.

* TOC {:toc}

## Storage Performance

I recommend using Diskspd for measuring the performance of the storage subsystem. I perform the tests using a proxy script, developed by David Klee, named [DiskSpd Batch](https://www.heraflux.com/resources/utilities/diskspd-batch/). It performs a batch of test cycles and exports its results to CSV.

## Quick SQL Instance configuration primer

Set .

[Download SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/en-us/library/mt238290.aspx)

## dbatools

[dbatools](https://dbatools.io) is a fantastic Powershell Module that, as described in its website, is a **free** PowerShell module with over 350 SQL Server administration, best practice and migration commands included.

[xSqlServer](https://github.com/PowerShell/xSQLServer)

## SQL Server Backup, Integrity Check, and Index and Statistics Maintenance

The Ola Hallengren SQL Server Maintenance Solution comprises scripts for running backups, integrity checks, and index and statistics maintenance on all editions of Microsoft SQL Server 2005, SQL Server 2008, SQL Server 2008 R2, SQL Server 2012, SQL Server 2014, and SQL Server 2016.

[Ola Hallengren SQL Server Maintenance Solution](https://ola.hallengren.com/)

## sp_Blitz – Free SQL Server Health Check Script

You’ve got a Microsoft SQL Server that somebody else built, or that other people have made changes to over the years, and you’re not exactly sure what kind of shape it’s in. Are there dangerous configuration settings that are causing slow performance or unreliability? You want a fast, easy, free health check that flags common issues in seconds, and for each warning, gives you a link to a web page with more in-depth advice.

[Brent Ozar's sp_Blitz](https://www.brentozar.com/blitz/)