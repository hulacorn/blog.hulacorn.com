---
title: Internal developer platform
date: 2021-04-20T19:30:00
author: podge-obrien
excerpt: Optimising ways of working may be cheaper than creating a whole platform
draft:
seo:
  title:
  description:
  image: 2022/04/platform.jpg
images: # relative to /src/assets/images/
  feature: 2022/04/platform.jpg
  thumb: 2022/04/platform.jpg
  slide:
tags:
  - Internal Developer platform
  - Producivity

---

## Introduction

There is lots of talk about internal developer platforms and the problems they solve. An example is they restore order to your microservices and infrastructure and enables your product teams to ship high-quality code quickly, without compromising autonomy(thanks backstage) or Relieve pressure on Ops by enabling developer self-service(thanks Humanitec).

Is there a more straightforward alternative way than adopting or building an internal developer tool?

How small independent teams can become unproductive and unknowledgeable.
Most small startups move fast and get things done because only a few engineers work on a problem. This small team has access and autonomy to build out what they want. The downside to this is as most startups are in survival mode, they can cut corners. Over time an operations team is introduced to ensure production stays running. They introduce rules and restrict access to the point it can be challenging to make changes. Lack of permissions and rights makes engineers frustrated. 

Companies introduce tooling to allow engineers to make changes to environments to solve this problem. However, engineers are still limited in what they can do, so for example, an engineer can use an internal tool to make a change to an existing service and nine times out of ten it works, then one time there is an issue with AWS, and your tooling gets "stuck". The only option now is to contact ops to fix it for you.  And by magic, everything works again. 
The engineer is not involved in fixing it, so they learn very little, and I have observed in many companies that engineers do not know how the underlying infrastructure works.

## An alternative approach
A way to mitigate tooling that allows engineers to do ops type work is to create an architecture that gives engineers ops responsibility at a low level for their service.
To do this requires a couple of pre-requisites. The org should have a domain-driven design approach to creating software and align their teams to this structure. They probably should also be using a cloud provider that can provide an abstraction similar to AWS subaccounts. If you have teams and software design aligned, and you can create subaccounts. Then these sub accounts can be controlled by the team designing and implementing the software. No more tooling like an internal developer platform is needed to generate the ability to modify the underlying infrastructure. From a governance perspective, tooling like Control tower allows you to audit what each team is doing in each sub-account.

## Epilogue 

Before going down the road of creating, and spending a lot of money, a complicated Internal developer platform, observe the organisations of teams, how they build software, optimise for them to be loosely coupled from other teams in the organisation when it comes to executing on building software.

