---
title: Anti-debugging and anti-tracing techniques part_2
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
## Introduction
PEB : 
 * PEB is high level user mode structure that holds some important information about the current process under it is field values-some field being structures
 themselves to hold more data. it is the address of the PEB structure in the TEB.ProcessEnvrionmentBlock member.The TEB structure is located at the start address of the
 segment memory pointed by the Fs segment selector, and the ProcessEnvrionmentBlock member is 30 Offset from the start 
 of TEB structure.
 ```
mov   eax , dword ptr fs:[18h]     ----> get address of TEB structure
mov   eax , dword ptr ds:[eax+30h] ----> get address of PEB structure
movzx eax , byte ptr ds: [eax+2]   ----> get value of PEB.BeingDebugged
test  eax , eax                    ----> if not zero, a Debugger has been detected
jne   debugger_detected
 ```

 * Every process has it is own PEB and the the Windows Kernel will also have access to the PEB of every user-mode process so it can keep track of certain
 data stored within it. so The Process Environment Block (PEB) is an important thing.

* The PEB structure comes form Windows Kernel although is accessible in user-mode. The PEB comes form the Thead Environment Block (TEB) which also
happens to be commonly referred to as the Thread Information Block (TIB). The TEB is responsible for holding data about the current thread – every
thread has it’s own TEB structure.



* Thread Environment Block or the Process Environment Block have been used for malicious purposes in the past but Microsoft 
has made a lot of changes over the recent years. in the past rootkits would inject a DLL into another running process. 
## synetx 
