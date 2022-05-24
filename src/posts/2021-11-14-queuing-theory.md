---
title: An Introduction to queuing theory in software development.
date: 2021-11-14T19:30:00
author: podge-obrien
excerpt: How queues affect the throughput of software teams.
draft:
seo:
  title:
  description:
  image: 2021/11/queue.jpg
images: # relative to /src/assets/images/
  feature: 2021/11/queue.jpg
  thumb: 2021/11/queue.jpg
  slide:
tags:
  - Developer tools
  - Internal Developer Platform
  - Software engineering

---

# Introduction

Queueing theory is the mathematical study of waiting lines or queues.
Queuing theory aims to design systems that serve stakeholders requests quickly and efficiently at a sustainable cost. 
At its most basic level, queuing theory involves an analysis of arrivals of requests, in my case, a software development team, and analysing the processes currently in place to process the requests. The end result is a set of outcomes on how to facilitate the requests from stakeholders.

# Why does queuing theory matter?

For software engineering, it demonstrates how fast a team can get something done and how much load the team is under.
Lots of companies don't invest in queuing theory. Instead, they prefer to manage timelines. Managers are taught how to create timelines and how to manage them. Mainly through the creation of detailed plans with dates that need to be met.
The more detailed the plans. The more unreliable the prediction of the end date. 
Why is it more unreliable? With detailed timelines comes a breakdown of work into tickets: the more detailed tickets. The more the timelines become strict, and developers must estimate when a ticket will be done. As developers are terrible at predicting, they start to add buffers to their work. With these added buffers, timelines become longer.
Queuing theory matters as it moves to focus from timelines to what slows the work.

# How do queues work?

If we have a software team with one engineer(as implausible as that may seem), Change requests take on average one hour to process. If the arrival rate is 9 for an 8 hour day, the person who requested the change will have to wait more, 1.125 days,  than a day before the request is processed. Why so long? even though it takes an about hour to process? Requests usually do not tend to come in an even distribution. If you added just one more engineer to the team, the processing time would come down significantly. The waiting time would be dramatically reduced to 0.08 days. Why less than a day? Because the probability of having at least one engineer free to process a request is considerably higher.

The above example uses an M/M/1 queue. An M/M/1 queue represents the queue length in a system having a single server, where a Poisson process determines arrivals and job service times have an exponential distribution.

The results for an M/M/1 queue with a request per  day is nine, and an engineer that can process eight requests per day:

- The engineer will be busy 113% of the time.
- The average waiting time will be -1.125 days.
- The average residence (waiting time + service time): 1 day.
- On average, the queue will have -10.125 requests in it.
- On average, the whole system will have -9 requests in the backlog.

if you increased the number of engineers to 2, then the results would look like this

- The server will be busy 56% of the time.
- The average waiting time will be 0.08 days.
- The average residence (waiting time + service time): 0.143 days.
- On average, the queue will have 0.723 units in it.
- On average, the whole system will have 1.286 units in.

# Managing queues
One of the most valuable tools for managing queues is the cumulative flow diagram (CFD). CFD's uses time as its horizontal axis and cumulative quantity as its vertical axis. We plot cumulative arrivals at a process as one variable and cumulative departures as a second variable. The vertical distance between the arrival line and the departure line is work that has arrived but not yet departed, thus, the instantaneous size of the queue. The horizontal distance between the arrival and departure lines tells us the cycle time through the process for an item of work. The total area between these two lines tells us the size of the queue.

We can also use this diagram to determine demand and capacity. The slope of the arrival line tells us the demand feeding into the queue. The slope of the departure line tells us the capacity of the process of emptying the queue. Thus, the CFD visually presents a great deal of information about our queue.


# Epilogue

Queues are the hidden source of most development waste. They are concealed from view in our management systems. Because they are composed of information, not physical objects, they are hard to spot in the workplace. Queues increase cycle time, expenses, and risk. Reducing queues is the key to improving software development performance.

We can affect queues by the structure of our queueing systems. In particular, when we share a common queue for multiple developers. 
Another way to affect queues is to remove queues altogether. Removing queues is a big part of cross-functional teams. If there is someone on the team dedicated to a function and no handover(logging a ticket and thus creating a queue) is needed, there is no queue to be managed.

It is not that simple; removing the queue needs to be coupled with other ways of work like Domain Driven Design to create domains with boundaries to isolate teams from an execution perspective.

I will talk about the other needed moving parts in future posts.