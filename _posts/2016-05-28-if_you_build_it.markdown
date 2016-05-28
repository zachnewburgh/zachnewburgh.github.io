---
layout: post
title:  "If you build it..."
date:   2016-05-28 19:01:57 +0000
---

Last week, I met one of my cousins to help him plan out a social media strategy for his four restaurants here in NYC, called ["The Grey Dog."](https://thegreydog.com/) They've got a relatively active presence on [Instagram](https://www.instagram.com/thegreydognyc/), but their approach could always use some improvement.

Little did I know that this conversation would lead into my Rails Assessment. No, I didn't build him a dashboard to track his social media profiles (though, perhaps I should). Instead, I built him something else.

During our lunch meeting, I noticed that he was manually assigning shifts to his employees, using a series color-coordinated charts and staff request forms. I turned to him and asked "So Dave, how long does this take you on a weekly basis?" He responded by saying that it took him several hours a week, and even despite this setback, the biggest issue was that often staff request forms get misplaced in the transfer to him or aren't turned at all – issues for which he gets blamed, which affect staff morale and his relationship with them.

Considering this, I offered to look into building him an app solution.

For me, the most challenging part of the project had to do with figuring out the infrastructure of the app. I knew that there needed to be a User, but I had some trouble figuring out with shfits should be a model of their own, and whether to include a Schedule, Request, and more. 

Then, came the associations. Would a User have many Schedules? If so, can a Schedule also have many Users? Need there be a join table user_schedules? In the end, I settled for:

```
User.rb
has_many :scheduled_shifts
has_many :shifts, through: :scheduled_shifts
has_many :requests

Shift.rb
has_many :scheduled_shifts
has_many :users, through: :scheduled_shifts

Request.rb
belongs_to :user

scheduled_shifts (table migration)
belongs_to :user
belongs_to :shift
```

With that figured out, I set off to build the app, using the [Devise](https://github.com/plataformatec/devise) gem and [Omniauth-facebook](https://github.com/mkdynamic/omniauth-facebook) with a lot of help from Will Schenk's incredible [walkthrough](http://willschenk.com/setting-up-devise-with-twitter-and-facebook-and-other-omniauth-schemes-without-email-addresses/). Initially, it took some fiddling to recognize how to use the parameters that Facebook provided in the authentication process, and then replace the User's attributes with them, where applicable. One interesting tidbit that I learned is that Facebook does not allow for the importing of phone numbers.

Realizing that users who hadn't signed up through Facebook authentication would be missing pictures of their own, so I implemented the [Paperclip](https://github.com/thoughtbot/paperclip) gem to allow them to upload their own avatars. I learned how to ensure that the images were all given the same size, so that oddly-shaped avatars wouldn't display my slick formatting (yes, I'm tooting my own horn here – sorry!).

In the end, the app turned out pretty well, but there's still a long way to go. I'm interested to automate the shift assignment process, so that the app will "auto-magically" create schedules based on a variety of parameters, without needing any human intervention. We'll soon see where things go, but for now, I'm pretty proud of the progress that I've made!
