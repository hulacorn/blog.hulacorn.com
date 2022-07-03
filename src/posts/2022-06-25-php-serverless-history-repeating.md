---
title: Serverless VS PHP shared hosting.
date: 2022-06-25T17:44:03.000Z
excerpt: A short rant on the similarities of PHP and Serverless
author: roberto-gambuzzi
draft: 
seo:
  title:  Serverless VS PHP shared hosting.
  description: A short rant on the similarities of PHP and Serverless
images: # relative to /src/assets/images/
  feature:
  thumb: 2022/06/serverless.jpg
  align: object-left # object-center (default) - other options at https://tailwindcss.com/docs/object-position
  height: h-64 md:h-1/3 # optional. Default = h-48 md:h-1/3
tags:
  - serverless
  - php
---

## Introduction

This is a short rant about history repeating itself.
- What WAS PHP shared hosting
- What is Serverless
- Similarities and differences

## What WAS PHP shared hosting?

For folks too young to remember, we can say that PHP shared hosting was how “the internet” started.
You wanted to create a website. You needed some space on the web, 
Somebody had servers (made of real metal) connected to the internet but it was too expensive to rent a whole server. So you were renting some hard disk space and the right to run PHP code (or PERL CGI scripts).
The PHP lifecycle was very simple.
- You invoke a PHP script in the browser.
- The web server decided, based on the host header, which folder to look in
- If the file was found, the PHP interpreter was called, parameters were passed as environment variables and stdin, and the script sent the resulting webpage to stdout.
- The PHP interpreter is closed as soon as the page script ends.

The deployment was via FTP, and you could change the code directly in ‘production’. Filezilla was your best friend.
If the machine your website was on, for some reason, had a problem,  providers moved your code to another machine. Cheaper providers were displaying “sorry”.
The evolution of this model made the PHP interpreted not startup and shutdown at all the calls but stay up and wait for another call for some time.
Security was terrible. No docker or VMs, so bad code could result in interferences between the websites hosted on the same machine.
Now you understand why there are so many PHP websites.


## What is serverless?

Cloud is somebody else’s computer. Serverless is when somebody else is responsible if the computer goes down.
Serverless isn’t magic where the software can run on nothing. There is always a CPU in a server executing your code; you can remove that variable from your infrastructure design (until you need to take that into account for other reasons, but this is a topic for another post).
Where is your code? Your code can be in different places, a zip file in a control panel, a zip file on S3, …
Can you edit your code directly online? Often yes.
My code starts based on an event, such as an HTTPS call.
Is my code immediately purged from the server it ran on? No, it is kept for some time to have faster startup time in case of other calls.
Is your code running alongside other people’s code in the same physical machine? Yes
Security is better now. The Linux kernel got better, and the jailing of the process now works better. Docker is helping a lot.

# Similarities and differences
- You can already see a lot of similarities, but let’s recap them:
- You can focus on writing your code
- You deploy your code by uploading it somewhere for some languages, without compiling it
- You can change your code directly online and in production, if you want (this is not a best practice, but not everyone is following best practices)
- Code executes alongside other people’s code
- Code can have some startup time that is not always predictable
- If the hardware fails in the middle of your code, it’s your problem (retry strategies aside)


## Conclusions

Running away from understanding technology is running in circles.
Don’t blame PHP for all the problems it created. There was an ecosystem of bad practices around it. PHP was giving too much freedom. But is the solution to remove the freedom or educate the software developers?