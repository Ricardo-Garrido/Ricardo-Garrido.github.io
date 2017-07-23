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
<pre><code class="language-powershell">
Get-EventLog -LogName *
Clear-EventLog system
</code></pre>

**Clear a single event log (e.g. system)**
<pre><code class="language-powershell">
Clear-EventLog system
</code></pre>

**Clear all event logs on the local computer**
<pre><code class="language-powershell">
Get-EventLog -LogName * | ForEach { Clear-EventLog $_.Log }
</code></pre>

or

<pre><code class="language-cmd">
wevtutil el | Foreach-Object {wevtutil cl "$_"}
</code></pre>

**Clear all event logs on a remote computer**
<pre><code class="language-powershell">
Get-EventLog -ComputerName $ComputerName -LogName * | ForEach { Clear-EventLog -ComputerName $ComputerName $_.Log }
</code></pre>

**Command Line**

**List event logs**
<pre><code class="language-cmd">
wevtutil el
</code></pre>

**Clear a single event log (e.g. system)**
<pre><code class="language-cmd">
wevtutil cl system
</code></pre>

**Clear all event logs**
<pre><code class="language-cmd">
for /f %x in ('wevtutil el') do wevtutil cl "%x"
</code></pre>