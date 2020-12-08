---
title: History of Reverse Engineering
keywords: sample
summary: "Looking back at the historical roots of reverse engineering."
sidebar: reveng_sidebar
permalink: history_of_reveng.html
folder: reveng
---

# The History of Reverse Engineering
![History Image](/images/egyptian-1823573_1920.png)


## Early Reverse Engineers
For as long as humans have been innovating, the most curious among us have been seeking to understand the inner workings of inventions. They use their access to the available data, concepts, or even physical structures to discover how something works. And as technology continues to progress there is an endless battle between the innovator trying to protect their ideas and the reverse engineer.

A famous case of early reverse engineering would be when Galileo simply heard about a telescope and from the information he gathered he figured out how to make one. The original inventor of the telescope, or at least the first to file a patent for it, was the Dutch eyeglass maker, Hans Lipperhey, who submitted his patent in 1608 [2]. But it wasn’t until it was in the hands of other curious inventors who took their understanding of how it worked and improved it so that it could be used in many different applications.

![Telescope](/images/skywatch-3384730_1920.png)

This example with Galileo is a demonstration of reverse engineering in a competitive sense. Take someone’s invention and figure it out just so that you can make your own, and possibly better, version. This has been a common practice throughout history and these days we have strict rules in place that make this more difficult, but not impossible. Now it is common to reverse engineer a competitor’s product and come up with a way to achieve the same results without infringing on their patent.


## A Reverse Engineering War Story
But of course reverse engineering has had more than just business uses. In war times it is common to try to figure out the weaponry of the adversary. Back in the early days of handheld weapons, a battle could be decided by whose armor was the strongest or whose blade was the sharpest. And if you could acquire your opponent’s weapon and figure out how to replicate it, or even what its weakness is, you would gain a significant advantage on the battlefield. 

A more recent example of wartime reverse engineering was in World War II when the United States developed a bomber airplane that was so advanced that it revolutionized engineering, aerodynamics, manufacturing, and electronics[3] in the United States. I’m talking about the U.S. Aircraft, the B-29 Superfortress.
 
The B-29 was developed and built in the 1940s and it was first used on June 5th, 1944 in a raid on Bangkok. The Russians were already aware of the B-29, but had instead been focusing their manufacturing efforts on smaller aircraft. Eventually the Soviets had requested some of their own B-29, but these requests were ignored. Even so, just 3 years later Russia was debuting the Tu-4 bomber at an air show in Moscow. And to the surprise of any westerner observer, it appeared to be a carbon copy of the B-29.

It turns out that the Soviets had received everything that they needed to reverse engineer the B-29 bomber on July 29th, 1944 when an intact B-29 was damaged and forced to land in Vladivostok. When it was all said and done four of the U.S. aircrafts had fallen into Soviet hands either by emergency landings in Russia or from salvaged parts from a crash. The Russian scientists, engineers, and technicians were able to quickly reverse engineer an entire bomber and put it into production. And even while the Tu-4 never made a significant impact in the war efforts, it definitely shocked the United States to see that the Russians had successfully reverse engineered a powerful weapon developed by the United States.


## Reverse Engineering Software
The invention of transistors was the start of it all. We had designed a simple electronic device that was capable of outputting a boolean state via electronic signal. Boolean logic was already a well studied concept and it seemed to happen very quickly that we had built networks of transistors capable of computations. The high level abstractions have made it easier and easier to utilize the computer but when it is always compiled into an instruction set that can be utilized by a processor.

There are too many examples to count, but there is one example that occurred early in the life of the internet and set precedence for the legal consequences of reverse engineering and what could be achieved. The Morris worm was created by the cryptographer, Robert Morris, who had discovered a buffer overflow in a network service and a hole in the debug mode of the Unix sendmail program. Just to see if he could do it, Robert Morris deployed the very first computer worm on November 2, 1988 [5] from a computer at the Massachusetts Institute of Technology just to see if he could actually do it. 

![Worm](/images/virus-2019480_1920.png)

At this point there were only about 60,000 computers connected to the internet and within 24 hours about 10% of them had been hit. The worm worked better than Morris could have imagined and the way that it hid itself and spread continuously ended up shutting down entire networks and making systems unusable. Even though it didn't actually directly damage any files, it still wreaked havoc on just about every Unix system on the internet.

This was intended as a simple prank, but it was determined that Morris had actually broken federal law due to the Computer Fraud and Abuse Act passed by Congress. Luckily for Robert Morris he was able to avoid jail time instead being assigned 400 hours of community service, probation, and a fine. Even though this punishment may not have been severe, it was a demonstration of the potential consequences of unauthorized computer access and  simultaneously a wake up call for the information technology industry to start taking security more seriously.


## Conclusion
There are countless examples of reverse engineering throughout history albeit by different names like theft or copying. Reverse engineering on its own is harmless but you can see how someone might take that knowledge and use it in a self-serving way.

As time progressed, technology became more complex and inventions were built on abstractions of electronics to enable developers. At the advent of software a new invention to figure out presented itself and with it came its own set of challenges. Early on developers basically had to write machine code that was very explicit in the way it would operate the transistor logic subsystems. Now software has been built on top of the machine code to greatly simplify common procedures with new tools.

Elliot J. Chikofsky and James H. Cross stated, "Reverse engineering is evolving as a major link in the software life cycle, but its growth is hampered by confusion over terminology" [4]. Moving forward we need to build upon the abstractions we have to create more robust and straightforward tooling for the next generation of reverse engineers.


## References
1. https://www.thesoftwareguild.com/blog/what-is-reverse-engineering/
2. https://www.space.com/21950-who-invented-the-telescope.html
3. https://www.airforcemag.com/article/0609bomber/
4. https://www.cs.cmu.edu/~aldrich/courses/654-sp05/ReengineeringTaxonomy.pdf
5. https://www.fbi.gov/news/stories/morris-worm-30-years-since-first-major-attack-on-internet-110218
