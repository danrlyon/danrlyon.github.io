---
title: Intro to Reverse Engineering
keywords: sample
summary: "A quick introduction to Reverse Engineering"
sidebar: reveng_sidebar
permalink: intro_to_reveng.html
folder: reveng
---

# Introduction to Software Reverse Engineering
Reverse engineering can mean different things when it comes to software. But generally speaking it is the process of taking software and converting it into something that is readable in some way and using that result to decipher the functionality of the software.


## Common Terminology
- **Disassembler** - software that converts machine language to assembly language
- **Decompiler** - takes an executable and converts it into a source file that can be recompiled
- **Packet Sniffing** - gathering network packets for analysis
- **Bus Analyzer** - a protocol analyzer for capturing communications directly off an interface bus
- **Stack Overflow** - occurs if the call stack pointer exceeds the bounds of the stack (more details in the [Ghidra](/look_at_ghidra.html) section
- **Obfuscation** - obscuring the source code of software to make it harder to understand
- **Code Injection** - introducing code into a vulnerable program in order to change its execution



## References
1. https://www.newworldencyclopedia.org/entry/Reverse_engineering#Reverse_engineering_of_software
2. https://learning.oreilly.com/library/view/practical-binary-analysis/9781492071204/
3. https://www.merriam-webster.com/

{% include links.html %}
