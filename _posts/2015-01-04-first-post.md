---
layout: post
title: Event Logs
image: ''
tags: []
date: 2016-06-14 00:00
subtitle: ''
bigimg: ''
show-avatar: false
comments: true
social-share: true
---
## List and Delete

#### **Powershell**

**List event logs**

```language-powershell
Get-EventLog -LogName *
Clear-EventLog system
```

**List event logs**

```language-powershell
Get-EventLog -LogName *
Clear-EventLog system
```
**List event logs**

```language-powershell
Get-EventLog -LogName *
Clear-EventLog system
```
**Clear a single event log (e.g. system)**

```language-powershell
Clear-EventLog system
```

**Clear all event logs on the local computer**

```language-powershell
Get-EventLog -LogName * | ForEach { Clear-EventLog $_.Log }
```

or

```language-cmd
wevtutil el | Foreach-Object {wevtutil cl "$_"}
```

**Clear all event logs on a remote computer**

```language-powershell
Get-EventLog -ComputerName $ComputerName -LogName * | ForEach { Clear-EventLog -ComputerName $ComputerName $_.Log }
```

**Command Line**

**List event logs**

```language-cmd
wevtutil el
```

**Clear a single event log (e.g. system)**

```language-cmd
wevtutil cl system
```

**Clear all event logs**

```language-cmd
for /f %x in ('wevtutil el') do wevtutil cl "%x"
```