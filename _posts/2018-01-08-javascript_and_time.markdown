---
layout: post
title:      "Javascript and Time"
date:       2018-01-08 15:41:24 +0000
permalink:  javascript_and_time
---


I wanted to write a bit more about some things I learned regarding Javascript and time. 

Firstly, it was a joy learning about asynchorous requests. For example, waiting until your document was ready to attach to your event listeners and run your Jquery, chaining a promise to your HTTP requests using a thenable object or even loading your rails asset pipeline properly so that your dependencies are loaded in the right order: time matters in Javascript, especially if your functions are not idempotent. 

Given that you have events, timer functions and X,Y coordinates for your mouse, Javascript felt more like playing in a self-created mini universe than other languages I've learned thus far. I realized you can make your own physics in Javascript: you can define [gravity/transfer of energy laws for bouncing balls](https://processing.org/examples/bouncybubbles.html) or slow down time with *setTimeout()*. That's pretty romantic, but I'm here to talk about something else I learned.

Since time is a human construct, it's imperfect and generally a huge pain to deal with when you're building a website that deals with handling visitors from all over the world or even simply multiple timezones. Even if you build a website for US customers, that's six time zones to consider. Beyond that, you have leap seconds, daylight savings and other ridiculous concepts. However, managing time across Rails/Javascript ended up being pretty straightforward. 

I was building a scheduling that had to parse time across these paths:
1) User enters in a time via a form in a browser 
2) The time is stored by Rails 
3) The time is then sent as JSON strings from the API to the front end
4) Javascript receives and parses the strings and displays them to you 

I didn't have time to test cross browser functionality so I just used Chrome. The first thing I had to do was convert all my date and time objects into datetime to make everything else easier. [ActiveRecord](https://apidock.com/rails/ActiveRecord/Base/default_timezone/class/) persists all datetime objects in UTC (Coordinated Universal Time). However, when I passed the datetime objects into the frontend via JSON, since I was in NYC, all the times were five hours behind.

I used the beautiful [MomentJS](https://momentjs.com/) for formatting purposes, but you can use plain Javascript itself too. Javascript by default gives you local system time instead of UTC, but it has this nice [UTC method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getUTCDate). 

Using MomentJS, I was able to turn my time strings (which arrived from the backend via JSON) into Javascript Date objects:

*scheduler.js*
```
this.start_date = moment.utc(attributes.start_date);
$("h1#title").append("Week of " + this.start_date.format('MMM D, YYYY')
```

For my scheduling app, it made sense to store and display my dates in UTC but with everything I learned, I do feel better prepared for when I may have deal with dates in a more complicated situation.
