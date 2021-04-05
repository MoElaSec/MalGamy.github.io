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
1--> Malware authors use Anti-Reverse Engineering Techniques a lot to impede the reverse engineering process of the malware and malware analyst runs malware samples in debugger to analyze the functionality and behavoir.

2--> the malware sample plays a lot of tricks to recognizes the dubuggers that are running with the help of Anti-reverse engineering techniques, when malware recognizes the dubuggers ,it hide the malicious functionality or it may terminate.

<!-- more -->

3--> I will presents several anti-debugging techniques that used on windows NT-base operationg systems.Anti-debugging techniques are ways for a program to detected if it runs under control of a debugger.they are used by commercial executable protector,packers and malicious software to prevent or is slow-down the process of reverse-engineering.

## IsDebuggerPresent

![400px-IsDebuggerPresent-example](https://user-images.githubusercontent.com/74544712/113621781-5b6d9d00-965c-11eb-965d-e13ae736897d.png)


### Description 

[IsDebuggerPresent()](https://docs.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-isdebuggerpresent), the most widely used anti-debugging method in Windows. However, here we will go a little bit into what isDebuggerPresent() does internally. As we can read in Microsoft’s documentation, it comes with a very simple prototype from Kernel32 library.

### Syntax

```BOOL IsDebuggerPresent();
```
### Return value
IsDebuggerPresent returns 1 if the process is being debugged or returns 0 if the process is not being debugged . This API simply reads the PEB!BeingDebugged byte-flag (located at offset 2 in the PEB structure Circumventing it is as easy as setting PEB!BeingDebugged to 0.
### bypass IsDebuggerPresent with x32dbg 
if you want your application never check it do this:
#### Step_1









