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
Malware authors use Anti-Reverse Engineering Techniques alot to impede the reverse engineering process of the malware .the malware analyst runs malware samples in debuggerto analyze the functionality and behavoir.the malware samples plays a lot of tricks to recognizes the dubuggers that are running with the help of Anti-reverse engineeringtechniques.when malware recognizes the dubuggers ,it hide the malicious functionality ot it may terminate.

<!-- more -->

I will presents several anti-debugging techniques that used on windows NT-base operationg systems.Anti-debugging techniques are ways for a program to detected if it runs undercontrol of a debugger.they are used by commercial executable protector,packers and malicious software to prevent or is slow-down the process of reverse-engineering.



