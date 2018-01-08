---
layout: post
title:      "Nested Active Model Serializers (has-many-through)"
date:       2018-01-08 14:56:36 +0000
permalink:  nested_active_model_serializers_has-many-through
---


I just finished modifying my Rails project to display a Jquery front-end. The rails side implementation made so much good common sense, just like rails ideas always do. I was worried about implementing a has-many-through via the Active Model Serializers but it was a piece of cake with rails. 

I have a scheduling app, so let's assume a schedule has many employees through shifts. In order to make the nested JSON come through, you just have to take a few steps:

1) Include the line `gem 'active_model_serializers'` in your Gemfile and run `bundle install`
2) Create a Seralizer for all three models, and your own custom ones if you'd like to exclude certain attributes. You can do this by typing `rails g serializer schedule` , for example. 
3) Go into the newly created serializer file and specify your attributes and associates as follows:

schedule_serializer.rb
```
class ScheduleSerializer < ActiveModel::Serializer
  attributes :id, :published, :start_date, :end_date
  has_many :shifts

end
```

You're almost done - you may notice that your JSON is only one layer deep. In my example, my JSON would be showing  a schedule and a given a shift, but not the associated employees. Here's the trick: in the controller itself (mine would be the show controller for schedule), you'll need to explicitly specify what to include as follows:

schedules_controller.rb:
```
  def show
    respond_to do |format|
      format.html { render :show }
      format.json { render json: @schedule, include: 'shifts,shifts.employee' }
    end
  end
```

That's it - this should create nested JSON three levels deep. Enjoy!
