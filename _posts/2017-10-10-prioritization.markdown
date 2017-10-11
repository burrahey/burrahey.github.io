---
layout: post
title:      "Prioritization"
date:       2017-10-11 01:24:28 +0000
permalink:  prioritization
---


Today, I built a project where I was the PM, designer, engineer and customer. I was to demonstrate that I had learned how to built a CRUD app using ActiveRecord and Sinatra (the most important line in my code was still `< ActiveRecord::Base`), so I built a budgeting app (I work in fintech). It was an exercise in patience and humility. 

First of all, even though there is a ton of CSS available on the web to grab and paste, it actually took a little bit of personalization just to make all the parts play nice and make sure basic things like buttons and nav bars were aligned. 

Moving on, the budgeting app itself was clunky. Off the top of my head, here are some changes I would have made: 
* Allow users to select discrete categories. If you had discrete categories, it would be simple to build a simple report to help you understand what you were spending the most amount of money on (and compare it to other users on the site). I could have managed this pretty quickly with some new columns in my Purchases table and a dropdown input form.
* Allow you to change your budget to be weekly or monthly - this could be accomplished by creating different methods in my model.
* Use a simple gem like Paperclip as a file upload tool to allow you to upload photos to accompany your purchases, which is [exceptionally easy](https://github.com/thoughtbot/paperclip) if you're using Rails. 
* Automatically grab your spending data from your credit card account. I don't know how to do this yet but you could probably use some integration partner like Yodlee. Of course if I did that, I 'd probably have to beef up my security rather than using a simple Bcrypt gem.

How do you know when to stop? As usual, I had way more ideas than I had time to improve upon my app. It was a quick and dirty project to get my feet wet and build a friendly CRUD application before I move onto to Rails. I'm so excited to get started on that and build some real apps. For now, this was a pretty fun exercise!


