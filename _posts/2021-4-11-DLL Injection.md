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
![image1](https://user-images.githubusercontent.com/74544712/114314260-42506a80-9afa-11eb-8416-17a22fa8271b.PNG)



DLL-Injection is the most common technique that used by malware to inject malicious code into other processes to evade detection, and every processes need to load dynamic link Libraries to work, So it became easy to load malicious code in legitimate processes

## Overview
The malware authors use some of Windows API functions that have a set of features, these features enable malware authors to attach and manipulate DLL-injection steps in legitimate processes in order to make a valuable attack.

In the first, malware determined the process that's been injected with malicious code, Malware follows some steps to get a list of running processes on a system.

* Takes a snapshot of the specified process, or all processes, also it used for enumerating heap, modules and thread states by using this process using by [CreateToolhelp32Snapshot](https://docs.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-createtoolhelp32snapshot) API function.

* After taking a snapshot of running processes, the malware will search for the target process to inject malicious code by two API functions, the first API function is called [Process32First](https://docs.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-process32first) which retrieves information about the first process encountered in a system snapshot
* The second API function is called [Process32Next](https://docs.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-process32next) which retrieves information about the next process recorded in a system snapshot.
* After the previous steps, the malware determine a process that used to the injection process.
* the malware gets the handle of the target process by calling OpenProcess.
* Get the process handle using the [OpenProces](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-openprocess) API.
* Attach to the proces using OpenProcess().
 ![attach](https://user-images.githubusercontent.com/74544712/114314699-05857300-9afc-11eb-970f-a6393ca98215.PNG)
 
* Allocate Memory within the process.

 ![image3](https://user-images.githubusercontent.com/74544712/114315008-5cd81300-9afd-11eb-89c3-9b9a0dc4ec67.PNG)

* Allocate Memory within the process
* Copy filename (path) to process memory using VirtualAlloc().

 ![image4](https://user-images.githubusercontent.com/74544712/114315053-75e0c400-9afd-11eb-83d3-1dc5d266f131.PNG)


* pass the address of LoadLibrary to one of these APIs so that a remote process has to execute DLL-injection technique.
 ![image5](https://user-images.githubusercontent.com/74544712/114315151-d8d25b00-9afd-11eb-9072-623bc3d9c037.PNG)




 



