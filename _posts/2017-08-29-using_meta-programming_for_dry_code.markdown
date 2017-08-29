---
layout: post
title:  "Using meta-programming for DRY code"
date:   2017-08-29 14:40:54 -0400
---


I just finished my first project, the CLI gem app and had so much fun doing it. I learned quite a bit, and I wanted to share my journey a little bit.

First, the inspiration: the San Diego Zoo website! Firstly, I should note that I am in no way affiliated with the good people of the San Diego Zoo. I do know that they have a killer instagram (seriously, go there only if you're ready to completely forego the next two hours of your life). Their site has a great list of all the animals in their zoo. You can click on each animal to find out more information, including taxonomic details, fun facts, conservation status and more. If you want to see it, check it out [here](http://animals.sandiegozoo.org/animals). 

Okay, this is pretty prime for scraping! It was pretty smooth sailing to build a simple "Animal" class and build the app and scraper - took less than an hour. However, it was slow as molasses because it was loading not only every single animal name, but all their details from all the individual web pages... yikes! 

I knew I wanted the CLI app to present a list of choices:

> Mammals
> Reptiles
> Birds
> Amphibians
> Arthropods
> Fish

This was a natural way to separate the creatures because this is how the San Diego Zoo [set up their site](http://animals.sandiegozoo.org/animals/mammals). 

I decided to make separate classes (programming construct) for each of classes (taxonomic reference) that all inherited from the "Animal" class, so I could only load only the classes the user wanted to see, and only the full details of the animals the user wanted to explore. How beautiful! My program reflected the taxonomic structure used to [organize biological creatures](http://basicbiology.net/biology-101/taxonomy/)! 

However, I quickly noticed this meant that I had to write a bunch of ugly, long case statements! For example, if the user selected "fish",  I had to scrape [the fish page](http://animals.sandiegozoo.org/animals/fish) and instantiate a bunch of fish instances. I intuitively realized that this is not very good practice. My code wasn't DRY. 

I biked home, showered, turned off the lights and went to bed. Then I had a thought: "Why not have one single class, "Animal", but have different methods to handle the different classes of animals? Then you could use the *send* method instead of having a bunch of case statements..." 

The *send* method is glorious. As someone who is a huge fan of Douglas Hofstader's [Gödel, Escher, Bach](https://en.wikipedia.org/wiki/G%C3%B6del,_Escher,_Bach), I immediately fell in love with metaprogramming (or what little I know about it). The send method lets you send any method to the receiving object, but you can use string interpolation to substitute a variable instead of a honest-to-god method name. Powerful stuff.

I immediately opened my eyes, turned on the lights, and changed my code. It took surpringly little time to write, but I had gotten rid of some 80 lines of code. I created a new branch and pushed it to Github so I could fix it in the next day on my lunch break. To my great surprise, it worked beautifully. My code wasn't fully of ugly, repititious code anymore! And I wasn't loading only the animals I absolutely needed, making my program fast, small and friendly! You gotta love those 1am inspirations - where do they even come from!? 

You can try my gem [here](https://rubygems.org/gems/SDzoo).
You can see my code [here](https://github.com/burrahey/SDzoo-cli-app). 

I'm looking forward to building more. It was electrifying the whole way through. I'm going to Portugal and Italy this weekend, though, for another kind of adventure. But I'll be back soon.. see ya then! Please do reach out if you have any feedback or thoughts! <3
