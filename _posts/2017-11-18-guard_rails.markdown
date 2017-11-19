---
layout: post
title:      "Guard[rails] "
date:       2017-11-19 02:32:34 +0000
permalink:  guard_rails
---


Wow, I haven't updated in a while. I was mainly heads down learning Rails, which culminated in [this project.](https://github.com/burrahey/scheduler)

![](https://imgur.com/a/KpFtX.jpg)

My team at work spends hours and hours painstakingly scheduling our employees, and I wanted to make the backbone of a scheduling app that could automatically generate the optimal shifts. There are a lot of scheduling apps out there, but none were perfect for what we needed. This was a problem ripe for automation.

This project laid the groundwork for such an app. I got learn about rails patterns, how to write DRY, readable code and better understand the "ruby" flavor. I feel like I'm really starting to understand the way to yield object oriented programming. Encapsulation is a beautiful thing, not only for testing, but reading and understanding code. 

The hardest part of this project was sitting down and planning it. Because I had created the schedules before by hand, my brain was absolutely buzzing with ideas on what kind of specifications to add: I was seeing the code for small pieces of the flow in my head. However, this is not the right way to approach a project because it's extremely overwhelming. I actually contracted strep throat during the "planning" phase, and had a moderate fever, so perhaps this exacerbated the mind clutter. 

I knew I had to start with my database, and go from there. I sketched out the classes that would dominate my app, and how they would relate to one another. From there, building my models, controllers and views became easy. Devise lets you develop a login feature (regular and oauth) within minutes.

From then on, I had made an individual list of tasks to complete, each framed as a user story. For example, I would write, "as an admin, I want to add new employees and sometimes assign them a shift". Once I built the feature, I realized that using the same partial for the "new" and "edit" views wouldn't work: as an admin, you want to add a password for a user when you create them, but not necessarily update the password every time you edit an employee. This made me pause: I wanted my code to be dry and look nice, but I also wanted a nice user experience: was there a way I could achieve both? I couldn't think of one that didn't result in two different methods, but I had to remind myself that user experience should trump trying to make your code pretty. It's a funny urge, but you have to resist that. 

It was immensely satisfying to eventually cross off all my user stories as 'done'. It also helped to make such a list because I was able to dive deep into building each story. As a code newbie, I often had to write the worst version of working code, and then have the pleasure of refactoring it. Once I was done, I had completely forgotten my vision for the product, but luckily I had my handy list. 

I always wondered how engineering managers at my company were able to do so many code reviews. I imagined their minds were sharpened like a new, expensive knife: I thought all code looks like those coding challenges that are designed to trick you, where you have to predict what the output is. I realized that having code like that in production is bad code! Your tests should catch off-by-one errors: human brains are best for capturing architectural issues. My brain can better answer: is this scalable? Am I taking advantage of encapsulation, polymorphism and other object oriented traits? If not, should I be using different tools to solve these problems? These are interesting questions, and I've only begun to discover [neat blogs](http://blog.steveklabnik.com/posts/2011-09-06-the-secret-to-rails-oo-design) about it. 

When you learn about the patterns Flatiron teaches you, it sounds like common sense. For example: your model should handle the data, your views (and helps) should handle display and your controller should be thin. However, it wasn't always the most obvious thing to do in practice. I would often have built my *create* or *update* actions only to find that I had to completely redesign my form_for in my view because I had way too much logic in my controllers. For example, I had a schedule, which had several shifts. You could add a shift either through an employee or through the schedule, but either way, they all had to talk to one another. It was a rewarding challenge to make that happen without repeating myself and making sure my code was readable. I'm sure I still have a lot to learn. I often found that stepping away and coming back to my code made previously frustrating parts of the flow much more obvious, as if my brain was slowly percolating the answer. I hope this means that my instincts are starting to form.

I also learned lots of fun little things. For example, I learned that working with dates is a pleasure with Ruby. Did you know that rails has a method called [*beginningofweek?*](https://apidock.com/rails/ActiveSupport/CoreExtensions/Date/Calculations/beginning_of_week) or that Ruby has a method called [*monday?*](http://ruby-doc.org/stdlib-2.2.0/libdoc/date/rdoc/Date.html#method-i-monday-3F). 

Additionally, I learned that if you want to do something, rails probably has already done it for you, but better. You want to validate that you're not adding two shifts for a single employee in a given day? You want to make sure that if an employee is deleted, all of their shifts are too? You want an instance variable set up ready for you to play with any time you reach a controller action with an :id variable in the URL? One line of code is it all it takes.

I found this on stack exchange, but I think this little snippet best exemplifies what I mean. Make your code general, repeatable and useful to the rest of the community:
```
  def check_and_display_errors(object)
    if object.errors.any?
      object.errors.full_messages.join(". ") + "."
    end
  end
```

What a beautiful little helper method. Any time an object has an associated error, it'll make the errors pretty and return them to you. Otherwise, it quietly returns a nil, not making a peep until it has to. 

I can't believe how much fun this was. I'm excited to hopefully contribute some useful things to open source communities in the future. Hopefully my scheduling app turns into something that reduces time and effort for my colleagues. 

Okay, that's it. Thanks for reading.

Arch
