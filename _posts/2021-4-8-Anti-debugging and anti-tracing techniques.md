---
title: Anti-debugging and anti-tracing techniques
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
# introducation 

* Malicious Software is able to detect if it is running under debugging environment because all debuggers have been attached to the process and it hides
the malicious behavoir to avoid detection from malware analyst if he attempting to debug the process.in this part i will describe common anti-debugging
techniques which are used to detect the presence of a debugger.

# NtGlobalFlag 
## Description 
* The NtGlobalFlag field of the Process Environment Block (0x68 offset on 32-Bit and 0xBC on 64-bit Windows) is 0 by default. Attaching a debugger doesn’t change the value of NtGlobalFlag. However, if the process was created by a debugger, the following flags will be set:

** FLG_HEAP_ENABLE_TAIL_CHECK    ---> (0x10)
** FLG_HEAP_ENABLE_FREE_CHECK    ---> (0x20)
** FLG_HEAP_VALIDATE_PARAMETERS  ---> (0x40)

The presence of a debugger can be detected by checking a combination of those flags.
## Example

![Anti-reverse-anti-debug-peb-ntglobalflag](https://user-images.githubusercontent.com/74544712/113925597-80dce100-97eb-11eb-9440-3d033b159cdd.png)

### 32Bit Process

```
mov eax, fs:[30h]
mov al, [eax+68h]
and al, 70h
cmp al, 70h
jz  being_debugged
```
### 64Bit Process

```
mov rax, gs:[60h]
mov al, [rax+BCh]
and al, 70h
cmp al, 70h
jz  being_debugged
```
### WOW64 Process

```
mov eax, fs:[30h]
mov al, [eax+10BCh]
and al, 70h
cmp al, 70h
jz  being_debugged
```
### C/C++ Code

```
#define FLG_HEAP_ENABLE_TAIL_CHECK   0x10
#define FLG_HEAP_ENABLE_FREE_CHECK   0x20
#define FLG_HEAP_VALIDATE_PARAMETERS 0x40
#define NT_GLOBAL_FLAG_DEBUGGED (FLG_HEAP_ENABLE_TAIL_CHECK | FLG_HEAP_ENABLE_FREE_CHECK | FLG_HEAP_VALIDATE_PARAMETERS)

#ifndef _WIN64
PPEB pPeb = (PPEB)__readfsdword(0x30);
DWORD dwNtGlobalFlag = *(PDWORD)((PBYTE)pPeb + 0x68);
#else
PPEB pPeb = (PPEB)__readgsqword(0x60);
DWORD dwNtGlobalFlag = *(PDWORD)((PBYTE)pPeb + 0xBC);
#endif // _WIN64
 
if (dwNtGlobalFlag & NT_GLOBAL_FLAG_DEBUGGED)
    goto being_debugged;
```
  
		
		


 
