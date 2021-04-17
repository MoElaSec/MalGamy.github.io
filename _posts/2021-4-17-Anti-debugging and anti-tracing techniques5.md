---
title: Anti-debugging and anti-tracing techniques part5
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
date: '2021-4-17 10-00-00 +0000'
toc: true
toc_label: Table of Contents
toc_sticky: true
---
# Introducation 

This article focuses on how to detect debug strings outputted by an application that calls Win32 API OutputDebugString.
And how bypass it using a debugger

<!-- more -->
#  OutputDebugString
![Outputdebuggingstring](https://user-images.githubusercontent.com/74544712/115109147-5f5ec080-9f74-11eb-86c2-280a0d115376.PNG)
## Description 
The Kernel32 outputDebugString () function that use to detect a debugger and explains different behaviour depended on the version of windows and it
Outputs a string to a debugger to attach it.
## Syntax
### c++
```
#include <Windows.h>

int main()
{
	// Make sure there isn't an error state
	SetLastError(0);
	// Send a string to the debugger
	OutputDebugStringA("Hello, debugger");
	if (GetLastError() != 0)
	{
		MessageBoxA(NULL, "Debugger Detected", "", MB_OK);
	}
	return 0;
}
```
### 32Bit Process
```
xor ebp, ebp
push ebp
push esp
call OutputDebugStringA
cmp fs:[ebp+34h], ebp ;LastErrorValue
je being_debugged
```
### 64Bit Process
```
xor ebp, ebp
enter 20h, 0
mov ecx, ebp
call OutputDebugStringA
cmp gs:[rbp+68h], ebp ;LastErrorValue
je being_debugged
```
## Return value
### Windows XP
* If the process is being debugged     ----> EAX will return 0 
* If the process is not being debugged ----> EAX will return 1 
### Windows Vista and above
* If the process is being debugged     ----> EAX will return 1
* If the process is not being debugged ----> EAX will return 0
changes between window Xp and Windows Vista or above are reason of changes in window internal

## Bypass OutputDebugString()
Before dipping into discussing how bypass this technique let us demonstrate ways to detect the debugger
### Frist way
* check the returned the value in EAX which depending on the Windows version
### second Way 
* GetLastError in Windows XP . If you don't have a Ring3 debugger, you'll get an error message that calls this API after the OutputDebugString.
 If EAX == 0 ---> a debugger has been detected 
### thrid way 
* Through SEH  Works in all Windows Versions from XP and
above, not tested in Windows 8.
### fourth way
* Olly Debugger would fail if you submit a string with the format % s % s % s. (tested up to OllyDbg v1.10).This is just a debugger-specific trick that takes advantage of a bug
and Olly Debugger will crash in this case 
