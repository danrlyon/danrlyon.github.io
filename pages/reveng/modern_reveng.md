---
title: Modern Reverse Engineering
keywords: sample
summary: "How does reverse engineering look now?"
sidebar: reveng_sidebar
permalink: modern_reveng.html
folder: reveng
---
# Modern Reverse Engineering


## Role in Cybersecurity
Reverse engineering is a two sided coin. The "good guys" will reverse engineer malware to figure out patches to protect devices. But the "bad guys" can use reverse engineering on software to find and take advantage of security flaws. In this way it can be said that reverse engineering is a core concept in cybersecurity as it is a necessary component on both sides. It is important that we are able to reverse engineer malicious software and that we are able to protect non-malicious software from exposing its vulnerabilities.

White hat hackers and security researchers will often take on the role of a cyber criminal. Companies and organizations will pay them to use their skills to find vulnerabilities in their software and hardware so that they can make improvements to protect themselves. This strategy has proven to be quite useful as the security researchers are often up to date on the latest tools and techniques that hackers use. This service has proven over time to be a successful method to test the security of your technologies.

It could be said that black hat hackers have it much easier. An organization needs to identify every vulnerability and protect themselves from all of the attack vectors. But a black hat hacker only needs to find one vulnerability in order to be successful. And all of the open source tools for reverse engineering are available to everyone. Reverse engineering can be a powerfully destructive tool in a hacker's arsenal.


## Legality
The legality of reverse engineering software is complicated and I would highly recommend legal consultation if you are unsure about whether you are allowed to reverse engineer a piece of software. The Electronic Frontier Foundation is a non-profit group that is defending digital privacy, free speech, and innovation. They have a lot of great resources if you have legal questions about white hat hacking. Below I will attempt to summarize some of the legal considerations of reverse engineering that the E.F.F. describe on [their fact page](https://www.eff.org/issues/coders/reverse-engineering-faq) [3], but it is by no means a definitive source and you should definitely research further if you plan to reverse engineer software.

The most important legal doctrines likely to affect reverse engineering:
"
- Copyright law and fair use, codified at 17 U.S.C. 107;
- Trade secret law;
- The anti-circumvention provisions of the Digital Millennium Copyright Act (DMCA), codified at 17 U.S. C. section 1201;
- Contract law, if use of the software is subject to an End User License Agreement (EULA), Terms of Service notice (TOS), Terms of Use notice (TOU), Non-Disclosure Agreement (NDA), developer agreement or API agreement; and
- The Electronic Communications Privacy Act, codified at 18 U.S.C. 2510 et. seq.
" [3]

Copyright law may limit your ability to reverse engineer. Copyright owners typically hold exclusive rights for making copies of their software. So in order to reverse engineer their software it must fall withing an exception granted by the copyright laws. Also executing the code has the possibility of raising copyright issues. Some courts have stated that causing code to be copied from disk to RAM might infringe if the copy on Ram is unlicensed. This all being said, a copyright owner can still give you the rights to reverse engineer their software.

One interesting recent example is *Blizzard v. BnetD*. The programmers of BnetD created an open source program that allowed people to play popular Blizzard games like World of Warcraft on servers other than the Battle.net service provided by Blizzard. Reverse engineering is a fair use under the federal copyright law, but unfortunately in this case Blizzard had explicitly stated in their End User License Agreement that reverse engineering was prohibited. The court determined that this click through contract was enforceable and therefore the programmers waived their rights when they clicked 'I Accept'.

Trade secret law is handled differently. Misappropriation of trade secrets is a civil and criminal offense. So generally speaking reverse engineering is allowed because "it is a fair and independent means of learning information, not a misappropriation." [3]. And information that is fairly and honestly discovered can be reported without violating trade secret law. But just like the aforementioned example of *Blizzard v. BnetD*, if contracts like an EULA or NDA are involved then it may be classified as misappropriation.

The big takeaway from my research is be careful. And if you are ever unsure consult a lawyer.



## Ways to Prevent Reverse Engineering
One of the more common ways to prevent reverse engineers from being able to reverse engineer your software is to make it unreadable or very difficult to read. This is called obfuscation.

The first and easiest step you can take to obfuscate your code is to change the name of every identifier in your code. By refactoring your code with nonsense function names, tracing the functionality and trying to discover useful pieces becomes more challenging. And with modern development tools this can be much easier to automatically search for the identifiers to replace it in every piece of the software. It is even possible to use characters that are unprintable so it becomes unreadable altogether. This does have its limitations as you may be importing outside libraries or software packages that would be difficult to make changes to.

To further hide the identifier names you can utilize string encryption with a key and decryption function inside of the code. By combining multiple techniques like these you can create very obfuscated code. Although you must take into account that the code not be obfuscated from the developer.

This strategy will make it much more difficult for the reverse engineer, but it is of course not completely protected. There are tools that can automatically deobfuscate programs. Even without the tools though, a determined reverse engineer would just need more time to put together the pieces on their own to figure out the functionality.

But while people are working to crack obfuscation, there are researchers also looking to make a completely unusable obfuscation. A group of researchers from IBM, the University of California, Los Angeles, and the University of Texas at Austin have come up with an obfuscation scheme that they call "mathematical obfuscation" [1]. The software is developed in a completely human readable format and the compiler first translates the code into an intermediate form before an obfuscating compiler takes this and turns it into a "Mathematical Jigsaw Puzzle" as UCLA computer science professor Amit Sahai called it.

The software is finally fed into a verifier program that puts all of the pieces back together and if it notices that something doesn't fit then someone must have tampered with the code. And if the verifier puts the pieces together successfully it will tell give you the correct output from the CPU.


## References
1. https://spectrum.ieee.org/computing/software/scrambled-code-keeps-software-safe
2. https://resources.infosecinstitute.com/topic/reverse-engineering-obfuscated-assemblies/
3. https://www.eff.org/issues/coders/reverse-engineering-faq
