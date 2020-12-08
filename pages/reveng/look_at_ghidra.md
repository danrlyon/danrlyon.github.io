---
title: A Look at Ghidra
keywords: sample
summary: "An open source reverse engineering framework used by the NSA"
sidebar: reveng_sidebar
permalink: look_at_ghidra.html
folder: reveng
---

![Hacker](/images/hacker-5471975_1280.png)

# Ghidra
[https://ghidra-sre.org/](https://ghidra-sre.org/)<br/>
A software reverse engineering (SRE) suite of tools developed by
NSA's Research Directorate in support of the Cybersecurity mission


## Introduction to Ghidra
In this section, we will introduce some reverse engineering topics as we dive right into a powerful reverse engineering framework developed by the NSA called Ghidra. The binaries for this software were released at an RSA conference in March 2019 and all of the software is free and open source. Now why in the world would the NSA release such a powerful tool and let it be used completely open source? This is because in the cybersecurity community the most powerful tool we have against malicious actors is our collaboration. By opening this toolkit to everyone, they are able to continuously update their software based on community feedback and with community support. The battle between black and white hat hackers is ongoing and rapidly changing so even though this might give a malicious hacker insight into the tools reverse engineers have, the benefit of being able to draw upon the community outweighs any advantage they may gain from having access to this tool.

Let’s get started. For this first example, we will have a look at the `a.out` Linux binary from crackmes.one since it is a very simple program. To follow along, go to [crackmes.one](https://crackmes.one/crackme/5f05ec3c33c5d42a7c66792b) and download the zipped binary. The password to unlock the archive is `crackmes.one`. Now we have our binary to reverse engineer, so let’s install Ghidra and have a look!


## Installing Ghidra
The system I am using is running Ubuntu 20, but there are [instructions]( https://ghidra-sre.org/InstallationGuide.html) for the most common platforms. All you need to do to run Ghidra is install the Java Development Kit (JDK) version 11 or greater. As this may change over time, it is always a good idea to check with the official documentation to be sure that you are using the recommended version.

Since I am on Ubuntu, this was relatively straightforward:
```bash
$ apt install openjdk-11-jdk
$ export PATH=<path of extracted JDK dir>/bin:$PATH
```

Once Java is installed and configured, you can simply download the archived software and extract it wherever you prefer this software to reside on your machine. Make sure to keep track of where it is installed as you will need to be in the installation directory to run the application. If you’re running Linux, just go ahead and navigate into the unzipped folder and run Ghidra with this command:
```bash
$ ./ghidraRun
```

Voila! You are now running NSA Reverse Engineering software. The software has a lot of useful built in documentation that can be used to help you in your reverse engineering projects.


## Starting a Project
Ghidra requires a project as all of the work must be done in the context of a project. Let’s make a project and call it `test-project`. Click *File -> New Project* to create a new project for this tutorial. You can create a non-shared project and store it wherever you would like on your computer. And if  you want to follow along exactly go ahead and name it `test-project`. 

To have a look at the `simple overflow` software go to *File -> Import File* and select the `a.out` executable from the simple overflow crackme that you downloaded previously. You will notice that the magic already begins as Ghidra recognizes the format and language of the executable. Also you will see some basic information about the executable being analyzed. 

Now double click on the file `a.out` to start analyzing it. Ghidra will notice that you have not run the analysis yet so make sure you do that when prompted.


## Using the Code Browser
{% include image.html file="code-browser.png" caption="Ghidra's Code Browser and Analyzer" %}

There are more features than I could possibly hope to cover in such a short tutorial, so I will just describe what we are using as we go. On the left there is a small box named *Symbol Tree*. A Symbol is a reference to data like imports, variables, or functions. Click on the Symbol named `main` as that tends to be the entry function for C applications like this one.

You’ll notice that the center *Listing* window highlights an area in the machine code. If you are new to reverse engineering then this may look like a whole bunch of gibberish but this is actually the raw instructions and data from the program. Since most modern software tends to be massive, you can imagine that you would use the Symbols to navigate to the sections of the code that are of the most interest instead of trying to wrap your head around the entire program.

To take a higher level approach to navigating the code, one can use the *Decompile* view on the far right. This view shows you actual code. If you are following along on your own computer, you may have noticed that the `main()` function is just a couple of lines to call the next function, `login()` and after it logs in, it will return. This is a pretty good indication that we should dig into the login function to understand what is really happening. 

Use the Symbol Tree view again to locate and click the login function. Since this is a simple program, it is easy to see that the interesting part of this code is the if statement. If the first unsigned integer stored in the variable puVar1 is equal to 0, you have logged in as admin. This gives us our objective. Things do get a little more technical at this point.


## Start Reverse Engineering
If you are familiar with C code, you can see in the Decompile view that the function asks the user for a password and allocates two buffers of 16 and 8 bytes on the heap using `malloc()` then stores the address to these buffers on the stack and sets the pointer to `1` which is the second `uint` position in puVar1. Since the memory allocation of `__s` was declared first, it is positioned in front of the `puVar1` memory allocation. 

This leads us to the `fgets` function that takes the input from the user and stores it in the allocated memory of `__s`. And comparing the possible input size, 160 bytes, to the size of the allocated memory for `__s`, 16 bytes, we find that if we input more than 16 bytes it will flow over into the next memory allocation in the heap, which happens to be the puVar containing the uid! We know that if the uid is equal to zero then you the program thinks you are admin based on the if statement responses. So if we write 16 bytes of anything to fill up the first buffer, follow it with another 8 bytes to fill up the first memory location, and finally write 0 to the second memory location we will have tricked the program into thinking that we are admin due to our user id.

As the name suggested, this really is a simple overflow example and now we have discovered how to break into the program by reverse engineering the functionality and finding a flaw in the code. If you are using Linux, here is how you can use `printf` to overflow the buffer and gain admin access:
```bash
$ printf "123456781234567812345678%b" '\x00\x00\x00\x00\x00\x00\x00\x00' | ./a.out 
enter password: uid: 0
you are logged in as admin
```


## Conclusion
We dove right into a very powerful reverse engineering tool so I want to take a moment to appreciate how much simpler Ghidra is to use than the old fashioned method of using the command line debugger. We literally were able to click an executable and Ghidra did the work of identifying what it was, analyzing the code, and even decompiling the code into something human readable. 

If you were following along, congratulations on a program well reversed. Most software will be nowhere near as simple to reverse engineer but you now have the tools to start that journey. 


## References
1. https://medium.com/swlh/intro-to-reverse-engineering-45b38370384
2. https://crackmes.one
3. https://ghidra-sre.org/InstallationGuide.html
