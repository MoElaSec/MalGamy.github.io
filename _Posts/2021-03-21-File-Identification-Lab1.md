---
title: File Identification 
layout: single
comments: true
share: true
related: true
author_profile: true
permalink: "/:title/"
toc: true
toc_label: Table of Contents
toc_sticky: true
categories:
- Malware_Analysis_Labs 
date: '2021-03-21 13:30:00'
tags:
- 
- Malware Analysis  
- Reversing
-File hash
-Strings
-File type 
---
<!-- more -->
## Introduction
I am so happy to write the frist Lab in my blog to help 
many people that begine in malware analysis around the world 
,i will write many labs and write every step in details
for lab...
## Samples 
* sample_1 [Here](https://app.any.run/tasks/328bfbaf-dd18-4460-a49d-ed842213be64/)
* sample_2 [Here](https://app.any.run/tasks/d3968bd2-3250-4bb0-8698-1800c46ea014/)
* sample_3 [Here](https://app.any.run/tasks/23d99d24-239a-4ad6-a809-1650f3779f91/)
## Goals 
The goals of this lab are able to build up hashes for samples ,identifying file types and extracting strings 
from this samples and use this information to build IOCS..
## Tools 
* PowerShell
* BinText
* Bstrings
* ssdeep
* FireEye Labs Obfuscated String Solver (FLOSS)
* Strings (Tsurugi Linux) – for offline usage
## File hahes 
It is very important to generate file hashes of the artifects that you deal with in this lab 
because it helps in your reports in malware analysis and i will use a couple of tools to generate
the hashes for all of the samples i will be dealing with.
let us start by using the CLI and generting the hashes for MD5,SHA1 and SHA256.
I will be using the PowerShell Get-FileHash cmdlet with the different algorithms, starting with (md5) in photo (1)

`Get-FileHash .\sampleA* -Algorithm MD5`


