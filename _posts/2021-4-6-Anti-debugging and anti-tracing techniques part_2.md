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
## Introduction:-
## Process-Environment-Block:- 
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
### synetx:-
* The PED structure id defined as follows:

![22596B3557A07D4826](https://user-images.githubusercontent.com/74544712/113758145-cded8400-9713-11eb-8895-4036255df003.png)
## BeingDebugged:-
### Description:-

* Instead of calling IsDebuggerPresent(), some packers manually check the PEB (Process Environment Block) for the BeingDebugged flag so when view PEB in WinDbg

```
kd> dt ntdll!_PEB
   +0x000 InheritedAddressSpace : UChar
   +0x001 ReadImageFileExecOptions : UChar
   +0x002 BeingDebugged    : UChar   ---> Indicates whether the specified process is currently being debugged
   +0x003 SpareBool        : UChar
   +0x004 Mutant           : Ptr32 Void
   +0x008 ImageBaseAddress : Ptr32 Void
   +0x00c Ldr              : Ptr32 _PEB_LDR_DATA
   +0x010 ProcessParameters : Ptr32 _RTL_USER_PROCESS_PARAMETERS
   +0x014 SubSystemData    : Ptr32 Void
   +0x018 ProcessHeap      : Ptr32 Void
   [SNIP]
   ```
   ### Return value:-
   * If byte ptr [eax+2] returns 1, it means the the program is being debugged and the jump at offset 0x4010D8 won't be taken.
   ![Anti-reverse-debugger-detection-peb-beingdebugged](https://user-images.githubusercontent.com/74544712/113761364-a7c9e300-9717-11eb-97ea-50f36ece6b44.png)

