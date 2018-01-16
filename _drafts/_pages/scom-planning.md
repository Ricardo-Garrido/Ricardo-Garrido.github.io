---
layout: page
show-avatar: true
title: SCOM - Planning
date: 2018-01-16 16:17:15 +0000
---
# Desabilitar monitores e regras dos Management Packs (para posterior ativação da base de monitorização)

 

Import-Module OperationsManager

 

\# Active Directory DS

\$MPNAME = ‘Active Directory Server Common Library’

\$OVerrideMPName = ‘Lactogal – AD DS Service’

\$MPSealed = Get-SCOMManagementPack -DisplayName $MPNAME

\$MPUnsealed = Get-SCOMManagementPack -DisplayName $OVerrideMPName

\$MPRule = get-SCOMRule -ManagementPack $MPSealed

\$MPMonitor = Get-SCOMMonitor -ManagementPack $MPSealed

Disable-SCOMRule -Rule $MPRule -ManagementPack $MPUnsealed

Disable-SCOMMonitor -Monitor $MPMonitor -ManagementPack $MPUnsealed

 

\# Active Directory FS

\$MPNAME = ‘Active Directory Federation Services 2012 R2’

\$OVerrideMPName = ‘Lactogal – AD FS Service’

\$MPSealed = Get-SCOMManagementPack -DisplayName $MPNAME

\$MPUnsealed = Get-SCOMManagementPack -DisplayName $OVerrideMPName

\$MPRule = get-SCOMRule -ManagementPack $MPSealed

\$MPMonitor = Get-SCOMMonitor -ManagementPack $MPSealed

Disable-SCOMRule -Rule $MPRule -ManagementPack $MPUnsealed

Disable-SCOMMonitor -Monitor $MPMonitor -ManagementPack $MPUnsealed

 

\# Active Directory CS

\$MPNAME = ‘Microsoft Windows Server Active Directory Certificate Services 2012 R2 Monitoring’

\$OVerrideMPName = ‘Lactogal – AD DS Service’

\$MPSealed = Get-SCOMManagementPack -DisplayName $MPNAME

\$MPUnsealed = Get-SCOMManagementPack -DisplayName $OVerrideMPName

\$MPRule = get-SCOMRule -ManagementPack $MPSealed

\$MPMonitor = Get-SCOMMonitor -ManagementPack $MPSealed

Disable-SCOMRule -Rule $MPRule -ManagementPack $MPUnsealed

Disable-SCOMMonitor -Monitor $MPMonitor -ManagementPack $MPUnsealed

 

\# DNS

\$MPNAME = ‘Microsoft Windows Server DNS Monitoring’

\$OVerrideMPName = ‘Lactogal – DNS Service’

\$MPSealed = Get-SCOMManagementPack -DisplayName $MPNAME

\$MPUnsealed = Get-SCOMManagementPack -DisplayName $OVerrideMPName

\$MPRule = get-SCOMRule -ManagementPack $MPSealed

\$MPMonitor = Get-SCOMMonitor -ManagementPack $MPSealed

Disable-SCOMRule -Rule $MPRule -ManagementPack $MPUnsealed

Disable-SCOMMonitor -Monitor $MPMonitor -ManagementPack $MPUnsealed

 

\# Hyper-V

\$MPNAME = ‘Microsoft Windows Hyper-V 2012 R2 Monitoring’

\$OVerrideMPName = ‘Lactogal – Hyper-V Service’

\$MPSealed = Get-SCOMManagementPack -DisplayName $MPNAME

\$MPUnsealed = Get-SCOMManagementPack -DisplayName $OVerrideMPName

\$MPRule = get-SCOMRule -ManagementPack $MPSealed

\$MPMonitor = Get-SCOMMonitor -ManagementPack $MPSealed

Disable-SCOMRule -Rule $MPRule -ManagementPack $MPUnsealed

Disable-SCOMMonitor -Monitor $MPMonitor -ManagementPack $MPUnsealed

 

 

\# Skype for Business Server 2015 Management Pack

\$MPNAME = 'Skype for Business Server 2015 Management Pack'

\$OVerrideMPName = 'Lactogal - Skype for Business Service'

\$MPSealed = Get-SCOMManagementPack -DisplayName $MPNAME

\$MPUnsealed = Get-SCOMManagementPack -DisplayName $OVerrideMPName

\$MPRule = get-SCOMRule -ManagementPack $MPSealed

\$MPMonitor = Get-SCOMMonitor -ManagementPack $MPSealed

Disable-SCOMRule -Rule $MPRule -ManagementPack $MPUnsealed

Disable-SCOMMonitor -Monitor $MPMonitor -ManagementPack $MPUnsealed

 

\# Exchange

\$MPNAME = ‘Microsoft Exchange Server 2013 Monitoring’

\$OVerrideMPName = ‘Lactogal – Exchange Service’

\$MPSealed = Get-SCOMManagementPack -DisplayName $MPNAME

\$MPUnsealed = Get-SCOMManagementPack -DisplayName $OVerrideMPName

\$MPRule = get-SCOMRule -ManagementPack $MPSealed

\$MPMonitor = Get-SCOMMonitor -ManagementPack $MPSealed

Disable-SCOMRule -Rule $MPRule -ManagementPack $MPUnsealed

Disable-SCOMMonitor -Monitor $MPMonitor -ManagementPack $MPUnsealed

 

# Exportar lista de monitores e regras por MP

 

\$mp = Get-SCOMManagementPack -Name Lactogal.Geral.SQL

Get-SCOMRule -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.Geral.SQL.Rules.csv

Get-SCOMMonitor -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.Geral.SQL.Monitors.csv

 

\$mp = Get-SCOMManagementPack -Name Lactogal.SAP.DEV.SQL

Get-SCOMRule -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.SAP.DEV.SQL.Rules.csv

Get-SCOMMonitor -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.SAP.DEV.SQL.Monitors.csv

 

\$mp = Get-SCOMManagementPack -Name Lactogal.File.Services

Get-SCOMRule -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.File.Services.Rules.csv

Get-SCOMMonitor -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.File.Services.Monitors.csv

 

\$mp = Get-SCOMManagementPack -Name Lactogal.SAP.PROD.SQL 

Get-SCOMRule -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.SAP.PROD.SQL.Rules.csv

Get-SCOMMonitor -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.SAP.PROD.SQL.Monitors.csv

 

\$mp = Get-SCOMManagementPack -Name Lactogal.Logical.Disk.Override

Get-SCOMRule -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.Logical.Disk.Override.Rules.csv

Get-SCOMMonitor -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.Logical.Disk.Override.Monitors.csv

 

\$mp = Get-SCOMManagementPack -Name Lactogal.Health.Library.Override

Get-SCOMRule -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.Health.Library.Override.Rules.csv

Get-SCOMMonitor -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.Health.Library.Override.Monitors.csv

 

\$mp = Get-SCOMManagementPack -Name Windows.LogicalDrives 

Get-SCOMRule -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Windows.LogicalDrives.Rules.csv

Get-SCOMMonitor -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Windows.LogicalDrives.Monitors.csv

 

\$mp = Get-SCOMManagementPack -Name Lactogal.Events.Override 

Get-SCOMRule -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.Events.Override.Rules.csv

Get-SCOMMonitor -ManagementPack $mp | Select DisplayName, Description, Name, Enabled, Target | Export-Csv -Path c:\\_sources\\Lactogal.Events.Override.Monitors.csv