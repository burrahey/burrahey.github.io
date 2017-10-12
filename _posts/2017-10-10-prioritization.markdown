---
layout: post
title:      "Prioritization"
date:       2017-10-10 21:24:29 -0400
permalink:  prioritization
---


Today, I built a project where I was the PM, designer, engineer and customer. I was to demonstrate that I had learned how to built a CRUD app using ActiveRecord and Sinatra (the most important line in my code was still `< ActiveRecord::Base`), so I built a budgeting app (I work in fintech). It was an exercise in patience and humility. 

First of all, even though there is a ton of CSS available on the web to grab and paste, it actually took a little bit of personalization just to make all the parts play nice and make sure basic things like buttons and nav bars were aligned. 

Moving on, the budgeting app itself was clunky. Off the top of my head, here are some changes I would have made: 
* Allow users to select discrete categories. If you had discrete categories, it would be simple to build a custom report to help users understand where they were spending the most amount of money on (and potentially see how they compare with other users on the site). I could have managed this pretty quickly with some new columns in my Purchases table and a dropdown input form. 
* Allow you to change your budget to be weekly or monthly - this could be accomplished by creating different methods in my model. We could even ask for your income, and based on other users on the site, recommend a buget for discrete categories!
* Use a simple gem like Paperclip as a file upload tool to allow you to upload photos to accompany your purchases, which is [exceptionally easy](https://github.com/thoughtbot/paperclip) if you're using Rails. 
* Automatically grab your spending data from your credit card/bank account. I don't know how to do this yet but you could probably use some integration partner like Yodlee. Of course if I did that, I 'd probably have to beef up my security rather than using a simple Bcrypt gem.

How do you know when to stop? As usual, I had way more ideas than I had time to improve upon my app. It was a quick and dirty project to get my feet wet and build a friendly CRUD application before I move onto to Rails. It was definitely one of the first times I remember feeling like I knew how to do a lot more, but only had enough time to complete the absolute bare minimum. I'd love to come back to this app if I had time some day.

How do you prioritize what you build? How do you make those decisions, especially when some things end up taking longer than expected? Get in touch if you'd like to chat more or simply add some pull requests [here](https://github.com/burrahey/budget-app).

