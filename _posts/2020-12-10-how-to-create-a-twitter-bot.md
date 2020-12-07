---
date: 2020-12-06T11:00:00.000Z
layout: post
title: How to create a Twitter bot
subtitle: Step by step creation of a twitter bot 
description: Step by step creation of a twitter bot
image: /assets/img/uploads/twitter.jpg
optimized_image: /assets/img/uploads/twitter.jpg
category: algorithms
tags:
  - bots
  - twitter
author: jaimedearcos
paginate: false
---
 
## 1. Request your developer account & create your first app

The first step of building twitter bots is to request access to developer features, this can be done easily 
in <a href="developer.twitter.com" target="_blank">developer.twitter.com</a>

Once you have fill the form to request access, you can log in the **twitter console** to retrieve the access keys:

![](/assets/img/uploads/twitter-dashboard.png)

![](/assets/img/uploads/twitter-tokens.png)

## 2. Set-up dev environment

We are going to use **Tweepy**, a python library to facilitate the communication with twitter API. To create the python 
environment and install Tweepy we need to execute the following commands:

```bash
python3 -m venv venv
source ./venv/bin/activate
pip install tweepy
```

After that we need to create our first python script to verify keys and environment (**test-credentials.py**)

```python
import tweepy

# Authentication
auth = tweepy.OAuthHandler("CONSUMER_API_KEY", "CONSUMER_API_SECRET")
auth.set_access_token("ACCESS_TOKEN", "ACCESS_TOKEN_SECRET")
api = tweepy.API(auth)

try:
    api.verify_credentials()
    print("Authentication OK")
except:
    print("Error during authentication")
```

Run the script by executing:

```bash
python test-credentials.py  
```

## 3. Playing with the API

Tweepy provides a really simple way to retrieve/create twitter information.

### **Retrieve your timeline**

```python
# ...

# Retrieve timeline
timeline = api.home_timeline()
for tweet in timeline:
    print(f"{tweet.user.name} said {tweet.text}")
```

### Publish a new tweet

```python 
api.update_status("Hello World from Tweepy")
```

### Retrieve info of any user

```python
user = api.get_user("JaimeDeArcos")

print("User details:")
print(user)

print("Last 20 Followers:")
for follower in user.followers():
    print(follower.name)
```

## Creating your first bot - Follow anyone who follows you

In this example we are going to create a bot which:
 - Retrieve the list of your followers
 - Check if you're already following them
 - If not, start following 

```python
import tweepy, time

# Authentication
auth = tweepy.OAuthHandler("CONSUMER_API_KEY", "CONSUMER_API_SECRET")
auth.set_access_token("ACCESS_TOKEN", "ACCESS_TOKEN_SECRET")
api = tweepy.API(auth, wait_on_rate_limit=True, wait_on_rate_limit_notify=True)

# Verify authentication
try:
    api.verify_credentials()
    print("Authentication OK")
except:
    print("Error during authentication")

while True:
    # Retrieve followers
    for follower in tweepy.Cursor(api.followers).items():
        if not follower.following:
            print("New Follower -> {}".format(follower.name))
            follower.follow()
    # Sleep for 2 minutes 
    time.sleep(120)

```

Simple, right? now you can improve the script with whatever use case you need. This script could be running locally or 
maybe you would deploy it on any provider of your choice _(AWS, Azure, google cloud...)_

---

## References & links

- [developer.twitter.com](developer.twitter.com)
- [Tweepy](https://www.tweepy.org/)
 
___



