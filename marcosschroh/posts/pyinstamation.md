<!--
.. title: Pyinstamation
.. slug: pyinstamation
.. date: 2017-12-14 14:29:19 UTC+01:00
.. tags: python, bot
.. category: bot
.. link: 
.. description: 
.. type: text
-->


## What is Pyinstamation?

Pyinstamation is a Python Bot for Instagram. I have developed this project with [Santiago Fraire](https://woile.github.io). You can take a look of the source code of Pyinstamation [here](https://github.com/dscovr/pyinstamation)

Features:

* You can configure **however** you want the bot.
* Works in Linux and MacOS.
* Works in Chrome and Firefox. 
* Upload pictures.
* Farm followers with the follow/unfollow technique.
* Like and comment by tags.
* Metrics persisted in db.
* Logging.
* Comment generator.

Everything looks great so far, but here, I want to show you about the results I had with this bot. 
I've been running this bot for 5 days in a row, once a day at the same time, and the results were quite good.

First I will show you the configuration I used for teh 5 days:

```yaml
username: yourusername
password: yourpassword
hide_browser: false
browser_type: chrome

posts:
  search_tags: ['hashtag_1', 'hashtag_2', 'hashtag_3', 'hashtag_4', 'hashtag_5',]
  ignore_tags: []
  posts_per_day: 100
  likes_per_day: 50
  like_probability: 0.5
  comments_per_day: 10
  comment_enabled: true
  comment_generator: true
  comment_probability: 0.5
  custom_comments: []
  total_to_follow_per_hashtag: 10

followers:
  follow_enable: true
  min_followers: 0
  max_followers: 0
  follow_probability: 0.5
  ignore_users: []
  follow_per_day: 50
  unfollow_followed_users: true
```


&nbsp;

**And here are the results per day:**

<table class='table table-bordered table-hover'>
	<thead>
		<tr>
			<td>Day</td>
			<td>Likes Given</td>
			<td>Likes Gain</td>
			<td>Comments made</td>
			<td>Comments gained</td>
			<td>My comments liked</td>
			<td>Mentions</td>
			<td><b>New users following</b></td>
			<td><b>New Followers</b></td>
			<td><b>Users Unfollowed</b></td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>15</td>
			<td>4</td>
			<td>10</td>
			<td>0</td>
			<td>1</td>
			<td>0</td>
			<td><b>15</b></td>
			<td><b>13</b></td>
			<td><b>0</b></td>
		</tr>
		<tr>
			<td>2</td>
			<td>19</td>
			<td>3</td>
			<td>10</td>
			<td>0</td>
			<td>5</td>
			<td>2</td>
			<td><b>23</b></td>
			<td><b>11</b></td>
			<td><b>0</b></td>
		</tr>
		<tr>
			<td>3</td>
			<td>13</td>
			<td>3</td>
			<td>10</td>
			<td>3</td>
			<td>7</td>
			<td>2</td>
			<td><b>15</b></td>
			<td><b>1</b></td>
			<td><b>15</b></td>
		</tr>
		<tr>
			<td>4</td>
			<td>47</td>
			<td>15</td>
			<td>10</td>
			<td>0</td>
			<td>1</td>
			<td>1</td>
			<td><b>47</b></td>
			<td><b>20</b></td>
			<td><b>10</b></td>
		</tr>
		<tr>
			<td>5</td>
			<td>46</td>
			<td>3</td>
			<td>10</td>
			<td>0</td>
			<td>3</td>
			<td>0</td>
			<td><b>10</b></td>
			<td><b>13</b></td>
			<td><b>15</b></td>
		</tr>
	</tbody>
</table>

&nbsp;

###Some numbers:


**New Followers:** 13 + 11 + 1 + 20 + 13 = 58

**New Users Following:** (15 + 23 + 15 + 47 + 10) - (0 + 0 + 15 + 10 + 15) = 70

*Is important to note that we only unfollow the users that have been  followed by the bot*

&nbsp;

Ok. So I was wondering whether the bot has achieved its goal or not? Well, my main goal was:
*have more followers with the follow/unfollow technique*

If we analize the bot from the goal perpective, we can say that it's working because I have more Followers, actually **47**.
Of course this result also dependes on the configuration that I've set: likes per days, comments, probabilities. etc.

But if we define the FR (Follow Rate) as followers/following, that is not good because I'm following more users than before, to be exact **70**. 
So, the FR = 58/70 = **0.8285**

We have more followers, but at the same time I'm following more users (bad rate) and I can guess that in the most of the cases a person wants to have a lot of followers but with a good rate (following << followers).

When we developed this project, Instagram had a limit of 7500 people that you can follow, so it’s means that if we run this bot indefinetely we can reach this number, but the good thing is that we don’t have any limit about followers.

Let’s suppose that we start with 100 followers and 100 users following.

Regarding the results that we had, we can say that:

Followers average per running: 58 / 5 = **11.6**

New Users Following per running:  70 / 5 = **14**  

If we do some math we can says that:

I will reach the 7500 limit of user following around: (7500 - 100) / 14 = **528 running**

I will reach the 7500 limit of followers around: (7500 - 100) / 11.6 =  **637 running**

So, I will have a FR = 1 around the running 637.

&nbsp;

![simple image1](/pyinstamation/running.png)

&nbsp;

If everything goes well and I keep running the bot I will have a FR > 1 after the running **637** times.

Probably is too much time If I run the bot once per day!!!

##Conclusion:

I think the bot has achieved the goal.
You will have to wait too much for a good RATE if you run the bot once a day with the same configuration.

What can you do to improve the Rate:

* Look at popular hashtags.
* Increase the amount of comments and likes per day.
* Do not Follow too much people per day
* Create posts per day.
* Increase comment and like probability. 
* Run the bot multiple times per day.
* Avoid unpopular hashtags

PD: If you have any issue or a new feature request don't hesitate to tell us!!. You can do it on our [pyinstamation](https://github.com/dscovr/pyinstamation) repo