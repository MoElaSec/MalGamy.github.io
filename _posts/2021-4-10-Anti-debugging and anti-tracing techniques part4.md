---
title: Anti-debugging and anti-tracing techniques part3
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
date: '2021-4-10 10-00-00 +0000'
toc: true
toc_label: Table of Contents
toc_sticky: true
---
# Introducation 

In last week i published isDebuggerPresent()[https://malgamy.github.io/Anti-debugging-and-anti-tracing-techniques/] technique which  function available in the kernel32.dll library.
This function is often used in malware to complexify the reverse engineering because it will take different paths in the program’s flow when the 
malware is analyzed in a user-mode debugger such as x32dbg and the most widely used anti-debugging method in Windows, Here i will
be going through anothor very commen technique that malware authors use it
,he CheckRemoteDebuggerPresent()[https://docs.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-checkremotedebuggerpresent] from kernel32.dll.

<!-- more -->
# CheckRemoteDebuggerPresent()

## Description 

* This Windows API can be used to detect if the calling process is being debugged through any debugger, but also if another process is being debugged.

![image(1)](https://user-images.githubusercontent.com/74544712/114262513-aa129280-99e0-11eb-94c3-901fa4300b60.PNG)

# Syntax
## MSDN

```
BOOL CheckRemoteDebuggerPresent(
  HANDLE hProcess,
  PBOOL  pbDebuggerPresent
);
```
## Debugger 

```
push pbDebuggerPresent_address ---> push the address of a 32 bit variable
push Process__handle           ---> push the handle to a process ( 1 for the calling process)
// call
CheckRemoteDebuggerPresent    ---> call the API
mov eax , pbDebuggerPresent_address ] ---> check the returned result in the variable
test eax,eax
jne _debuggerfound            ----> if not zero, a debugger was found.
```
## C/C++ Code
```
BOOL bDebuggerPresent;
if (TRUE == CheckRemoteDebuggerPresent(GetCurrentProcess(), &bDebuggerPresent) &&
    TRUE == bDebuggerPresent)
    ExitProcess(-1);
```
## x86 Assembly
```
lea eax, bDebuggerPresent]
    push eax
    push -1  ; GetCurrentProcess()
    call CheckRemoteDebuggerPresent
    cmp [bDebuggerPresent], 1
    jz being_debugged
    ...
being_debugged:
    push -1
    call ExitProcess
```
## x86 Assembly
```
  lea rdx, [bDebuggerPresent]
    mov rcx, -1 ; GetCurrentProcess()
    call CheckRemoteDebuggerPresent
    cmp [bDebuggerPresent], 1
    jz being_debugged
    ...
being_debugged:
    mov ecx, -1
    call ExitProcess
```






