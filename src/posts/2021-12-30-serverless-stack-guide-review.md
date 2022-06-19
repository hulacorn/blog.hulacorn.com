---
title: Serverless Stack Guide Review
date: 2021-12-30T19:30:00
author: podge-obrien
excerpt: Does it live up to the promise that it allows you to make changes and test your Lambda functions live?
draft:
seo:
  title: Serverless Stack Guide Review
  description: Does it live up to the promise that it allows you to make changes and test your Lambda functions live?
  image: 2021/12/guide.jpg
images: # relative to /src/assets/images/
  feature: 2021/12/guide.jpg
  thumb: 2021/12/guide.jpg
  slide:
tags:
  - Serverless-stack
  - Build-in-public
  - Software-engineering
  - Startup

---

# Introduction
I have been using AWS Lambda since it was made available in 2015. I used to gravitate to companies that worked in this space. Back then I would jump on anything new when working with Lambdas, monitoring tools like [IOPipe(Acquired by New Relic)](https://newrelic.com/blog/nerd-life/iopipe)or frameworks like [Stackery](https://www.stackery.io/) or [serverless.com](https://www.serverless.com/).

# Developer experience I look for
I use lambdas in most places as they have so many integrations and have advantages on cost. However, I stopped evangelising Lambdas and frameworks like serverless.com for large projects with many developers as I found it hard to have decent developer experience.
My definition of a decent developer experience is something that I 
- can clone a repository to my local machine, run npm install and then run one command to start all parts of the app.
- can see that has a small feedback loop after making changes(no more than a second or two)

It does not sound like much, but it was a chore back then to find anything close to this experience. A lot of it was running lambdas from a CLI on my machine. Nothing that a frontend could call an API to then call that lambda, not without deploying it first, which took some time, not a lot of time but enough that it was not a great experience.

 A lot has changed since then. Recently I started looking at the landscape of what's out there regarding developer experience and developing using Lambdas.

# Serverless Stack
Among all my searches, Serverless Stack (SST), whose tagline is "a framework that makes it easy to build serverless applications", was the one that stood out. It's a YCombinator backed starter from the Winter 2021 cohort. I was attracted to the promise of their framework "that allows you to make changes and test your Lambda functions live."

They have created a [guide](https://serverless-stack.com/#guide) on using their open-source framework for serverless. The [guide](https://serverless-stack.com/#guide) talks you through creating a Note-taking app.
It is a typical tutorial pattern, lots of sites have tutorials on how to make Note taking apps. There are various levels of implementation, from simple takes notes frontend and storing notes in local storage to full end-to-end implementations deployed with a public interface(app store or website).

Serverless-stack is on the upper end of the scale. The guide does a complete end to end implementation that is secure and deployed to a publicly available website. They do take it to the next level, in my opinion. 

### This brings me to what I like about the Serverless Stack guide.

- What I like the most about Serverless Stack is it allows you to test your lambda application live. The fact I could execute one command, npx sst start. It would start a session that allowed me to create an app and run it "locally", the WOW factor for me was making a change to my lambda code locally and seeing it reflected in the lambda in AWS a second later. It was this near-instant update happening, and the fact that I have my react site running locally made everything feel like it was a local development experience.
- Serverless-Stack does an Infrastructure as code approach, which allows you to define your Infrastructure and deploy it. Most other guides out there get you to deploy manually. It's a lot of click ops to set up what you need to set up. Setting up manually might sound great as you learn the moving parts of React or AWS, but once you have done it, you want to get away from that work model. You may want to create your app and let the complications disappear in the background. 
- Along with Infrastructure as code that most other tutorials do not cover, they also did a section on monitoring and debugging using [Sentry.io](https://sentry.io) and [Seed.run](https://seed.run/)(This product is by the same folk who make the Serverless Stack framework)
- There is a significant focus on CI/CD and development as a team. This is one thing I always found lacking in other tools or guides and was hugely refreshing. The CI/CD tool is [Seed.run](https://seed.run/)(This product is by the same folk who make the Serverless Stack framework)
- Beyond the initial guide, they provide a massive amount of more reading material so you can get a better understanding of the different moving parts like Dynamodb or Postgres. On top of that, there is an extra credit section to improve things like adding social logins to your site.

### Things I found annoying.
- The Infrastructure as code is still cloud formation under the hood. I have a personal disliking of cloud formation due to the nature of it getting "stuck." 
- They use Cognito for authentication. I do not know one person/company that operates this service. I would have preferred and implementation of Auth0 or something similar.


# Epilogue 
Serverless Stack is the framework I wished existed a few years back for people who wanted to develop in Lambdas. I was able to set up my note taking app on one of my own URL's [memyselfandi.io](https://memyselfandi.io/) quickly using the guide provided. I found it lived up to its promise of making it easy to change Lambdas live. My next step is to use this framework for a more production-related project to see if it holds up. Subscribe to my newsletter at [padraigobrien.com](https://padraigobrien.com/) or follow me on Twitter [@padraigobrien](https://twitter.com/padraigobrien) to see how I got on