---
title: Your code doesn't have to change the world.
date: 2022-07-10T17:44:03.000Z
excerpt: An article that walks through initial interest, research, and execution of a simple, somewhat useless Python/Selenium bot.
author: adam-mann
draft: 
seo:
  title:  Your code doesn't have to change the world.
  description: An article that walks through initial interest, research, and execution of a simple, somewhat useless Python/Selenium bot.
images: # relative to /src/assets/images/
  feature:
  thumb: 2022/07/bot.jpg
  align: object-left # object-center (default) - other options at https://tailwindcss.com/docs/object-position
  height: h-64 md:h-1/3 # optional. Default = h-48 md:h-1/3
tags:
  - python
  - bot
  - learning
---

## Introduction
Until recently, I'd only ever worked on or created projects that were focused on web development. I started off with Javascript/React, made a slew of standard portfolio projects, and then took up Python/Django after landing a job.

What I love about Python is the ease and extent to which you can write code that interacts with different kinds of digital objects, whether that's through web-scraping, browser automation, image manipulation, etc. You can do great things with Javascript, but web scraping is typically not as simple of a process in that language since the scraping itself must be done through NodeJS. With well-documented Python packages such as requests, Beautiful Soup, and Selenium, you can accomplish an incredible amount of web scraping with not very much code.

# Automation with Python

When I heard about Automate the Boring Stuff with Python, I knew right away I'd found a new rabbit hole to fall into. This book covers a lot of ground, but I've found myself most interested in the sections on web scraping, sending emails programmatically, and working with spreadsheets. There's something adventurous and powerful about going out on the open web and grabbing what you want. 

After finishing the chapter on web scraping, I decided to tackle one of the challenges. The prompt that stuck out for me was this one:

```md
2048 is a simple game where you combine tiles by sliding them up, 
down, left, or right with the arrow keys. You can actually get a fairly high 
score by repeatedly sliding in an up, right, down, and left pattern over and 
over again. Write a program that will open the game at 
https://gabrielecirulli.github.io/2048/ and keep sending up, right, down,
and left keystrokes to automatically play the game.
```

 # I set some rules for myself:

- I would not look at anyone else's code who may have completed the project.

- Rather than go back and look at old code I'd written, I tried to use only documentation and Google to accomplish what I wanted. Doing the leg work of finding and reading docs, Stack Overflow, etc. when you don't know something can be half the battle in many projects.

- I wanted to use OOP. Coming from a Javascript/React with Hooks background, that style of writing code is something I'm weak in. This seemed like a simple enough project to get some experience writing in that style.

- Since this was intended to be a portfolio project, I wanted feedback on the code. After finishing, I would post the project in a couple of places and ask for contributors to the Github repo.

# My process

With most projects, I first try to get a very small thing to work, then I usually functionize that and move on to the next step. I keep doing this until I have something that works, and then I clean up and start adding features.

My first step was getting the game's start button from the DOM and figuring out the syntax for sending key presses. My first lines of code looked something like this:

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys

driver = webdriver.Firefox()
driver.get('https://play2048.co/')

container = driver.find_element(by=By.TAG_NAME, value='html')
start = driver.find_element(by=By.CLASS_NAME, value='restart-button')
start.click()
container.send_keys(Keys.ARROW_RIGHT)
```

After the page loaded, I could see the tiles shift to the right! This was my starting point. Once I had that, I completed a hard-coded game loop that sends a key press, checks for game over, and prints a score at the end of the game loop. I made notes to myself on what to add and what to functionize. The rest of the code ended up looking like this:

```python

high_score = 0

#TODO list:
# - Prompt users to play a certain number of rounds. If not specified, do infinite until user presses CTRL + C
# - Use OOP to break up bot functionality
# - Display some kind of stats at the end?

# TODO: remove hard-coded cycle and accept input prompt
for i in range(4):
    game_over = False
    # TODO: functionize start cycle
    start = driver.find_element(by=By.CLASS_NAME, value='restart-button')
    start.click()
    while game_over == False:
        # TODO: functionize (and maybe randomize) key entries. consider adding a small sleep between each key press
        container.send_keys(Keys.ARROW_RIGHT)
        container.send_keys(Keys.ARROW_DOWN)
        container.send_keys(Keys.ARROW_LEFT)
        container.send_keys(Keys.ARROW_UP)
    # TODO: functionize game-over loop
        try: 
            driver.find_element(by=By.CLASS_NAME, value='game-over')
            game_over = True
            # set high score to best score
            best_score_value = driver.find_element(by=By.CLASS_NAME, value='best-container')
            high_score = best_score_value.get_attribute('innerHTML')
            retry_button = driver.find_element(by=By.CLASS_NAME, value='retry-button')
            retry_button.click()
        except:
            continue

print(f'Best high score: {high_score}')
```

# Refining and adding features

The code above would run and play out a set number of rounds, but I wanted the finished product to be more dynamic, and I wanted the code to be much better organized. Below are some of those additions.

- I wanted the bot to be able to loop for a set number of rounds using command line arguments (or infinitely if no arguments were specified). If the user enters a number, the game will loop for that number of rounds. Otherwise, it will play forever:

```python
 def game_loop(self):
        try:
            if self.rounds:
                for _ in range(self.rounds):
                    self._round_loop()
            # infinite game loop     
            else:
                for _ in count(0):
                    self._round_loop()
            self.driver.quit()
        except:
            self.driver.quit()
            print('Game stopped!')
```

- Because Selenium is extremely quick in how it manipulates the browser, I gave the user the option to set the speed of the game when the program started:

```python
   def _speed_prompt():
        speed_choices = ['Fast', 'Medium', 'Slow']
        sleep_times = [.1, .3, .5]
        questions = [
            inquirer.List('speed',
                          message="Choose a speed for the bot:",
                          choices=speed_choices,
                          )]
        answer = inquirer.prompt(questions)
        idx = speed_choices.index(answer['speed'])
        game_speed = sleep_times[idx]
        return game_speed
```

- I decided I wanted the keypresses to be randomized, so I created an array with all possible key presses and used a random number generator to determine what keypress to use on each move cycle:

```python
self.keys = [Keys.ARROW_RIGHT, Keys.ARROW_DOWN, Keys.ARROW_LEFT, Keys.ARROW_UP]

   def _key_press_loop_random(self):
        i = random.randrange(0, 4)
        self.container.send_keys(self.keys[i])
        sleep(self.game_speed)
```

# Asking for help

Since this was my first time working substantially with OOP in Python, I wanted to get feedback on how I could improve. I posted a link to my repo in a Python projects subreddit and asked for feedback. Within a few hours, I had a few solid suggestions for changes that would make my code more readable. One commenter even submitted a PR to the project, which I ended up merging.

# Where to go from here:
At the moment, the bot follows a simple, randomized loop that doesn't attempt to maximize the final score. Adding a pathfinding algorithm that calculates the optimal direction to slide the tiles would make the project much more interesting. So, if you're looking for a potentially challenging DS&A project, feel free to make a PR to the repo [here](https://github.com/aemann2/2048bot)



