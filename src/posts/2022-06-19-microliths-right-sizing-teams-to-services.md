---
title: Monolith, Microservices and Micro Monoliths, A story on rightsizing teams to services.
date: 2022-06-19T17:44:03.000Z
excerpt: How going blindly into microservices can introduce too much complexity and lead to disaster.
author: roberto-gambuzzi
draft: 
seo:
  title: Monolith, Microservices and Micro Monoliths, A story on rightsizing teams to services.
  description: How going blindly into microservices can introduce too much complexity and lead to disaster.
  image: 2022/06/microservice.jpg
images: # relative to /src/assets/images/
  feature:
  thumb: 2022/06/microservice.jpg
  align: object-left # object-center (default) - other options at https://tailwindcss.com/docs/object-position
  height: h-64 md:h-1/3 # optional. Default = h-48 md:h-1/3
tags:
  - microservices
  - monolith
  - micromonolith
---

## Introduction

This article is about the path from a monolith to a disaster.

We will not talk about serverless. That will be the topic of another article soon.

We will not discuss the UI blocks, like backend-for-frontend (BFF) or single-page applications (SPA). Instead, we will focus on

- What is a monolith?
- What is a microservice?
- What happens to your data when you split the monolith?
- Microservices madness.
- What do we mean by micromonolith (or microlith)

# What is a monolith?

<br> ![what is a monolith](../../../../assets/images/2022/06/1.svg)

By monolith, we mean a single piece of software that handles all our business logic and functionalities, typically contained in a single source code repository. Let’s use e-commerce as an example.

Monolithic e-commerce would be composed of a database running somewhere, plus a machine or a fleet of machines with a load balancer on the front, all running the monolithic software. Based on the language, there can be different ways of running this monolith.

If we use PHP, the monolith is a bunch of files in a folder with apache or Nginx in front of it, calling and starting the correct PHP file at every call. If we use Java, there could be a Tomcat or JBoss application server with an always-running process responding to requests.

The static part of the e-commerce, like images and CSS, could be served by the application or directly by the server running it.

The problem the companies have after a while running this monolith for a time is typically three:

- The different teams (payments, orders, fulfilment, marketing, etc.) start to develop features. When there is a need to deploy those features, there is a need for coordination between the teams. A change by a team can break some other team functionalities, and a bug in a feature can block the release cycle, putting teams on hold, not for their fault. Nobody will be happy.
- Build times. The growth of the monolith implies more unit tests (this can be solved partially by parallel testing), and longer build time (here again depends on the language, PHP has no build time, GO has a fast compiler, Java… is Java). More dependency can come in place and make the build pipeline brittle. Nothing is worse than an unstable build pipeline.
- Performance. We have two problems with performance here. One in the code and one in the database.
    - The code: e-commerce has multiple endpoints and multiple functionalities. Not all of them have the same traffic. There will be many people looking at products, fewer putting something in the cart, fewer going to the checkout and fewer making a payment. But every request carries the same base layer of configuration, information, routing rules, etc. We could address some of these problems with smart code, lazy loads and so on, but the issue remains. We don’t want the payment to fail because the CPU is busy serving ten thousand product pages. Adding more instances and a load balancer(LB) can mitigate the problem for a while. We could use some instances only for serving product pages and others only for serving the checkout pages (deploying the same monolith but smartly routing the traffic, but a regular LB is not doing this), but this still goes downhill.
    - The database: The database handles everything. Users list, products, carts, orders, etc. A first solution is to add read replicas to the master database. This means that the code must be smart and tell apart calls that are read-only (like browsing the products list) and calls that are updating the state, like adding to a cart or making an order. This again can buy the company some time while a better strategy is planned.


# What is a micro-service?
Micro-services come to the rescue. More or less. I think they got misunderstood like the agile movement has been (this is a story for another time).

<br> ![what is a monolith](../../../../assets/images/2022/06/2.svg)

The solution to the problem with monolith was to split the software into smaller pieces of code.

This improved some aspects of development:

- Now teams can move at different velocities. They can deploy when they want. They are responsible for their failed deployment.
- It forces teams to agree on a common interface between the different services, which was probably not done in the monolith because of poor design and poor implementation.
- The teams can choose different languages, letting them use the right tool for the right job.
- Build times are faster because the codebase to build is smaller. There is no need to rebuild the cart code if we update the order service.
- Every single µ-service can scale differently. We could have ten instances of the product information µ-service but only two for the payment µ-service allowing us to optimise the costs of the infrastructure.

We introduce new problems by adopting µ-services:

- Dependencies handling: we will probably have some piece of code needed by different microservices (for example, authentication libs or financial libs) that before in the monolith was shared and called but now needs its repository, build pipeline, and so on.
    - Private dependency repository: do we want to share those internal libraries with the world? Probably not, so we need to think of an internal store for the built artefact of our libraries (think of a private npm repository)
- Network: the network is not free (and I don’t talk about money)
    - Latency: what is in a monolith is just a function call that can take nanoseconds. In a microservices environment, it takes at least 10+ milliseconds. The speed of a signal in an optical fibre can’t be faster than what physics lets it be.
    - Errors: the network can fail. Your call will fail at some point, you have to consider retries, but not too aggressive retries or this could make the recovery of the failed service impossible. I often saw a service that was restarting, being hammered with retries, and failing to restart, going in a loop of sadness.
- More sophisticated deployment tools are needed: now you have to handle deploys of different services at different versions, and everything needs to be working during the deployment (and obviously after the deployment). A rollback strategy is required to handle the rollback of different services at once.
- Teams can’t make breaking changes to the current contract at the API version, so more effort in the API versioning and deprecation paths are needed.
- This splitting of functionalities can get out of control, having one µ-service for one endpoint (think of a single AWS lambda serving a single API gateway endpoint). Now the company has 200 µ-services and ten teams. This is a problem.
- Monitoring and alerting must be centralised somewhere (another µ-service? or 3rd party service like Splunk/datadog?) and tagged so we can tell the log apart.

# What happens to your data when you split the monolith?

<br> ![what is a monolith](../../../../assets/images/2022/06/3.svg)
<br> ![what is a monolith](../../../../assets/images/2022/06/3b.svg)

It is not very useful to split the monolithic codebase but not to break up the monolithic database. Can it be a good first step for migration? We think extracting one functionality at a time, both code and database, are better. If the rest of the remaining monolith still needs the extracted table, it is better to put a read-only copy of them in the original DB and keep them updated with a script. If the monolith stills need to write to the table, then you didn’t extract the correct code from the monolith.

At the end of this process, there will be some micro-services without the need for DBs and some services that will only have access to a single database, and no other microservice will access this database.

If the orders microservice needs some user data, the orders service will call an API on the user microservice. This way, the user microservice can handle the cache with its logic.

# Microservices madness.
<br> ![what is a monolith](../../../../assets/images/2022/06/4.svg)
The microservice pattern is going too far. The last extreme has a microservice that handles only one API request, like a single endpoint.

Now add to the scenario that the company has a limited number of engineers and teams. A team now has to handle multiple microservices. The onboarding is longer. More credentials are needed, and pipelines could be different and thus imply more onboarding time.

The complexity goes up. The debugging is now more difficult.

The permutation of possible calls is growing exponentially with any new microservice added.

At one point, you will need to switch from an HTTP call with JSON to a binary protocol like Protobuf or cap-n-proto. This will require more tooling to be able to debug.

More tooling leads to more difficult onboarding and recruiting. Fewer engineers will be able to understand your infrastructure. If you are Google, that is fine, but if you are a smaller company, think twice.

Think of taking the original monolith and adding a decorator to all your functions that can fail 0.1% of the time and send you to an old version of the code 5% of the time. This simulates your network and deploy pipeline.

You could object that all the pipeline problems and onboarding problems could be solved by making the pipelines always the same, but this would go against one of the requirements: using different technologies for the right job.

# What do we mean by micromonolith (or microlith)
<br> ![what is a monolith](../../../../assets/images/2022/06/5.svg)

To look back to the reasons that led to the monolith split.

It is good to split it if you need to scale but look at the number of your engineers.

If you have twenty engineers, probably they will split into four or five teams. You want them to be independent. Every team can handle two or three services. Now we have the micro(mono)lith.

A micro(mono)lith is a big chunk of code with its data store layer and its team that will handle them. The team itself will model the domain in this service. Domains are already typically divided by team. When a domain splits between two teams, the split never works as planned.

Can two different domains stay inside the same micro(mono)lith? Yes, you just need to write good code. And what if the engineers do not write good code? If you need to split this piece of code someday, it is easier to break something in two than to split a monolith into one hundred services.

For security and performance, we can have more than one micro(mono)lith per team, but let’s keep this number limited.

Let’s add an access layer and an internal-facing service layer.

This is still a reasonable amount of services to handle per team.

<br> ![what is a monolith](../../../../assets/images/2022/06/6.svg)

The objective is to keep teams independent from each other without too much useless complexity.


# Epilogue
We just warn people against the abuse of the microservice model. Not everyone is Netflix or Google. Not all companies need to solve problems of the same size.

And what if my company grow!?! You will need to hire more software engineers. In any case, this means you can handle more micro(mono)lith. At one point, you will have just microservices. But you will be able to grow organically without imposing on yourself too much complexity to start with.