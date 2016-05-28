---
layout: post
title:  "These little town blues..."
date:   2016-05-11 09:52:47 -0400
---

Noisy neighbors, slow-to-repair supers, lazy landlords? Being here in New York, I've heard it all. That's why I spent a few days building a website that would allow people to rate their apartments – it's akin to Yelp for real estate rentals!

The trickiest part for me was figuring out the architecture of the website. I knew that I'd need to create a User model, and figured that I'd need to build a Review, but I didn't anticipate needing an Apartment from the outset. 

After realizing that individual review show pages wouldn't be *that* helpful, I decided to elaborate on my simple has_many/belongs_to relationship between User and Review; thus, developing a has_many :through relationship between User, Review, and Apartment. I subsequently removed the index and show pages for Review.

Another obstacle had been with my implemention of `flash`. I'd read in on Stack Overflow that rack-flash is no longer compatible with Sinatra, so I had to find a substitute. Initially, I'd attempted to set up `sinatra-flash` but ended up relying on `rack-flash3`, in order to provide me with the needed support.

If you're curious to learn more about my work-in-progress, feel free to take a look at the video posted [here](https://drive.google.com/open?id=0B-xsMiWmDyyzRVF6V0puVjc0djA)!


