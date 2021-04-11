---
title: process Injection Techniques 
layout: single
comments: true
share: true
related: true
author_profile: true
permalink: "/:title/"
tags:
- malware analysis 
- anti-debugging
- anit-tracing
categories:
- Articles
date: '2021-4-4 11-00-00 +0000'
toc: true
toc_label: Table of Contents
toc_sticky: true
---
# Process Injection

process injection is a common Evasion tactic that used by malware authors in order to hide malicious code into legitimate processes and execute it on a system, also allow malware to gain access to other processes for example Banjan trojan access on web browser to steal credentials. 

Attackers use process injection as a type of anti reverse engineering to evade detect by both malware analyst and solutions, communicate with systems across c2 and in this post i wiil write about DLL injection technique. 

<!-- more -->

# DLL-injection 
![khVpE](https://user-images.githubusercontent.com/74544712/114314130-b76f7000-9af9-11eb-9b33-92f9236702b1.png)


DLL-Injection is the most common technique that used by malware to inject malicious code into other processes to evade detection, and every processes need to load dynamic link Libraries to work, So it became easy to load malicious code in legitimate processes

## Overview
The malware authors use some of Windows API functions that have a set of features, these features enable malware authors to attach and manipulate DLL-injection steps in legitimate processes in order to make a valuable attack.

In the first, malware determined the process that's been injected with malicious code, Malware follows some steps to get a list of running processes on a system.

* Takes a snapshot of the specified process, or all processes, also it used for enumerating heap, modules and thread states by using this process using by [CreateToolhelp32Snapshot](https://docs.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-createtoolhelp32snapshot) API function.

* After taking a snapshot of running processes, the malware will search for the target process to inject malicious code by two API functions, the first API function is called [Process32First](https://docs.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-process32first) which retrieves information about the first process encountered in a system snapshot
* The second API function is called [Process32Next](https://docs.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-process32next) which retrieves information about the next process recorded in a system snapshot.
* After the previous steps, the malware determine a process that used to the injection process.





