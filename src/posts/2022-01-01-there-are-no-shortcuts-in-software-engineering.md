---
title: There are no shortcuts in software engineering
date: 2022-01-01T19:30:00
author: podge-obrien
excerpt: The erosion of best practices and good disciplines in software development causes the creation of poor software.
draft:
seo:
  title: There are no shortcuts in software engineering
  description: The erosion of best practices and good disciplines in software development causes the creation of poor software.
  image: 2022/01/shortcut.jpg
images: # relative to /src/assets/images/
  feature: 2022/01/shortcut.jpg
  thumb: 2022/01/shortcut.jpg
  slide:
tags:
  - Software-engineering
  - Management

---

# Introduction
The erosion of best practices and good disciplines in software development causes the creation of poor software. Poor software can be bug-ridden, have a terrible user experience and cause a lot of rework for engineers over time.

# Discipline
The teams who focus on creating a good developer experience and building quality into the software process will win in the long run. They do this by creating a discipline in how they engineer the software.

The disciplines I have observed over the last two decades from some of the best teams I have worked with are as follows.

- The best engineers know that they need to increase their cadence of what they need to deliver over time and that this increased cadence does not come through increased working hours. It comes by decreasing working hours through automation. It is similar to the capitalism quote, "if you are not growing, you are dying", which is the same with releasing. If you are not increasing the frequency of your releases, then you are dying.
- The engineer should build their service for the next engineer coming after them. The system the engineers operate in should reward this thinking.
- Engineers are constantly conversing with the person who wants the features built. The requester should describe to the engineer in detail the behaviours. The worse way to have this conversation is via comments on Jira. The best is face to face.
- Talking about what behaviours users expect to see in a feature is much better than discussing the implementation of the solution. It keeps everyone using the same language and overcomplicating things.
- The engineer should be interested in their domain. They should know what drives the customer to use their product and help solve their customer's problems. 
- The requester should answer any question the engineer has, and if they cannot, the engineers keep digging until they get the answer.
- The engineer should document any git repository they own well.
- The code should be checked out from git and run locally without much time figuring out how.
- There should be good testing in place. This means going beyond unit tests and doing integration and end-to-end testing. Running tests locally is a must. Fixture data is a must.
- Engineers should not copy and paste from some arbitrary source, either Github/ Stackoverflow or other random sources.
- Engineers should set up good monitoring to align with what our customers value. If they value a sign up a new user and that flow is ten steps to 10 different systems, then you monitor those ten steps, how long they take, failures that happen etc. if it's a simple request-response flow, then you observe that(context is critical), read [this book on implementing serivce levels](https://www.oreilly.com/library/view/implementing-service-level/9781492076803/) to get a better insight  
- Implement according to what is expected. If any API says its content-type is JSON and returns a string, it creates confusion and work. If you consume this API and "Workaround" the flaw, you are in an imperfect system as an engineer. I liked this [book on building APIS people wont hate](https://apisyouwonthate.com/) for some good ideas 


# The exception to the rule
I would point out something that may seem contrary to the above. All the disciplines don't have to happen before you release a feature all of the time. It is OK to be scrappy and get the feature shipped before documenting it or thoroughly testing it. Sometimes you may need to learn something and the best way is to ship something. However, if you do not go back and finish documenting or testing after the fact, that isn't good. If that is all you do, then that is terrible.

# Epilogue
if you follow the above disciplines consistently, you increase the happiness, effectiveness and impact a team can have in an organisation.
You will also create a team that is super predictable to forecast when they will ship features