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

```
Get-EventLog -LogName *

```

**Clear a single event log (e.g. system)**

```
Clear-EventLog system

```

**Clear all event logs on the local computer**

```
Get-EventLog -LogName * | ForEach { Clear-EventLog $_.Log }

```

or

```
wevtutil el | Foreach-Object {wevtutil cl "$_"}

```

**Clear all event logs on a remote computer**

```
Get-EventLog -ComputerName $ComputerName -LogName * | ForEach { Clear-EventLog -ComputerName $ComputerName $_.Log }

```

**Command Line**

**List event logs**

```
wevtutil el

```

**Clear a single event log (e.g. system)**

```
wevtutil cl system

```

**Clear all event logs**

```
for /f %x in ('wevtutil el') do wevtutil cl "%x"

```