---
layout: post
title:  Ruby in the Sky with Diamonds
date:   2016-05-05 19:14:57 +0000
---

The last few days have been full of excitement – I published my very first [gem](https://rubygems.org/gems/top-headlines) and it already has 450 downloads in only 55 hours!

It all started out with an idea to create a gem that would scrape the top headlines from major news sources and print them to the command line. I figured that it'd be worthwhile to allow users to open the headlines in the browsers, too. Here are a handful of videos that documented my program: [one](https://drive.google.com/open?id=0B-xsMiWmDyyzcGk3MmlTc0xQOXM), [two](https://drive.google.com/open?id=0B-xsMiWmDyyzNDFyS01icFMtams), [three](https://drive.google.com/open?id=0B-xsMiWmDyyzU0VGNGJ5QkpaOUU), and [four](https://drive.google.com/open?id=0B-xsMiWmDyyzbEdzX0ZlOVcwM2M).

Some of the major challenges included nested [if and while staments](https://github.com/zachnewburgh/top-headlines-cli-gem/blob/master/lib/top-headlines/cli.rb), using [dynamic css selectors](https://github.com/zachnewburgh/top-headlines-cli-gem/blob/master/lib/top-headlines/source.rb), and formatting the output to maximize the user experience. 

The greatest obstacle for me, as it turned out, was figuring out how to publish the gem and release new versions. Surprisingly, I had some trouble finding documentation that was clear, especially since my search terms seemed to return instructions on how to update gems that were previously installed, rather than update the version of one's own gem.

After much tinkering, I discovered the process is relatively simple: (i) update the [.gemspec](https://github.com/zachnewburgh/top-headlines-cli-gem/blob/master/top-headlines.gemspec) with the name, version, authors, email, summary, short description, homepage, and license ; (ii) enumerate the version in [version.rb](https://github.com/zachnewburgh/top-headlines-cli-gem/blob/master/lib/top-headlines/version.rb); (iii) add and commit the changes; and, (iv) either type `gem push [.gem filename here]`in the command line to publish or type `rake release` in the command line to release a new version. Here's a helpful [guide on publishing](http://guides.rubygems.org/publishing/); those on releasing new versions seem to be more scarce.

At this point, the gem scrapes from 16 different news sources, and I'll soon be adding some non-U.S. media, as well as improved functionality.


