---
bigimg: ''
comments: true
date: 2017-07-23 17:59
image: ''
layout: post
show-avatar: true
social-share: true
subtitle: List, search, delete,...
tags: []
title: Event Log Manipulation
---
## List and Delete

**List event logs**
<pre><code class="language-powershell">
Get-EventLog -LogName *
Clear-EventLog system
</code></pre>
or
<pre><code class="language-powershell">
wevtutil el
</code></pre>

**Clear a single event log (e.g. system)**
<pre><code class="language-powershell">
Clear-EventLog system
</code></pre>
or
<pre><code class="language-powershell">
wevtutil cl system
</code></pre>

**Clear all event logs on the local computer**
<pre><code class="language-powershell">
Get-EventLog -LogName * | ForEach { Clear-EventLog $_.Log }
</code></pre>
or
<pre><code class="language-powershell">
wevtutil el | Foreach-Object {wevtutil cl "$_"}
</code></pre>

**Clear all event logs on a remote computer**
<pre><code class="language-powershell">
Get-EventLog -ComputerName $ComputerName -LogName * 
| ForEach { Clear-EventLog -ComputerName $ComputerName $_.Log }
</code></pre>