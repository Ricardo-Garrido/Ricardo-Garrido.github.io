---
layout: post
show-avatar: true
comments: true
social-share: true
title: Event Log Manipulation
date: 2017-07-23 18:38
image: ''
tags: []
subtitle: ''
bigimg: ''
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