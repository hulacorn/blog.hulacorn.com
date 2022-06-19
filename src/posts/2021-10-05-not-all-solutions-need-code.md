---
title: Not all problems require a software solution
date: 2021-10-05T19:30:00
author: podge-obrien
excerpt: Sometimes less is more when it comes to code
draft:
seo:
  title: Not all problems require a software solution
  description: Sometimes less is more when it comes to code
  image: 2021/10/code.jpg
images: # relative to /src/assets/images/
  feature: 2021/10/code.jpg
  thumb: 2021/10/code.jpg
  slide:
tags:
  - Software-engineering
  - No-code


---


# Introduction
We are addicted to software. Google "how to solve X?". You will get numerous results on how to solve X. This translates to how software teams work. Most companies spend vast amounts of time developing a process or a tool to solve a problem when none should exist.

# What happens?
A straightforward question you should be asking yourself when someone says, let us build X to solve Y. think to yourself the following
What is the actual problem?
How does it manifest itself?
How are we proposing to solve it by building software?
Can we replace the software solution with a simple no software solution?
Why do I bring this up? Because often I end up using half working, half thought out, abandoned projects at work that are not supported or documented. 

# What should I do instead?
Well, that depends. The scope of most of the observations I have had on this problem is simple. e.g. Operations keep getting inundated with tickets to deploy software. They build a bespoke tool that allows the engineer to do your deploys. Great, problem solved. If you want to fix a broken build(because builds do break), we will modify our tool to let them do that when we have the time. Of course, we got other work prioritised. Rinse and repeat a few times, and you have a tool that may not be fit for purpose. 
The solution is simple, give the engineers the ability to deploy using the ops way of deploying, any builds fail, that's ok, provide engineers with the same rights the ops people have to fix the problem.

# Issues with that solution
Oh My God, we cannot do that. Engineers could bring down Production, well, to be fair, so can Operations. But it is an appropriate point. How do you limit the blast radius and design a solution that gives engineers an isolated environment, one that if it blows up, it does not blow up 100% of Production? A sub-account in AWS per team would be a good approach.

# Epilogue 
Not everything is a software solution. Sometimes no-code solutions are better and introduce far fewer bugs, trust your engineers to do the right thing.