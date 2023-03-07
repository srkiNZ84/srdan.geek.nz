+++ 
draft = true
date = 2023-03-08T11:39:42+13:00
title = "du equivalent in powershell"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

```
Get-ChildItem . | % { $f = $_; Get-ChildItem -r $_.FullName | Measure-Object -Property Length
 -Sum | Select @{Name="Size";Expression={$f}},Sum}
```
