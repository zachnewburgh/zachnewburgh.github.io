---
layout: post
title:  "remote: false"
date:   2016-06-15 10:26:39 -0400
---


Adding jQuery, Active Model Serializers (AMS), and JavaScript Model Objects to [shiftr](https://github.com/zachnewburgh/shiftr/tree/master/app) were very helpful in diving deeper into some interesting functionality.

To complement the shift request and scheduling sections of the app, I added a series of pages for the employer to record notes for the staff. Acting much like blog, employees can scroll through various posts and leave comments where appropriate.

Implementing jQuery and AMS were relatively easy, but it was the last of the bunch that had me stumped. I had initially used the `remote: true` macro, discussed by David Heinemeier Hansson in his blog post titled, *Server-generated JavaScript Responses* [[link]](https://signalvnoise.com/posts/3697-server-generated-javascript-responses). 

This enabled me to use ERB in my .js files since the remotes brought the AJAX calls directly back into my Rails stack. All that I needed to do was to create [action].js.erb files, so that my remotes would know the right file from which to gain instruction. Suddenly, I had the ability to call my class variables and add Ruby logic to my JavaScript – what a breeze!

However, the trade-offs related to this simplified pattern set me back by several days.

A common criticism that I've heard of the Ruby community is that not enough of its users know what lies under the hood. The complexity of the code that lies under the `remote: true` macro is lost by the shortcut. Its magic took out some important features.

The most significant for me was the ability to convert a request into a JSON response to then construct a JavaScript Model Object. I had reviewed several Stack Overflow posts that had made the claim that it could be done by adding `format: :json` [[link]](http://stackoverflow.com/questions/9583380/rails-3-remote-form-how-do-i-specify-the-content-type), but all that I could return were errors. In the end, I had to settle for the more archaic AJAX call, specifying my `dataType`, and manually manipulating the response. 

Though it took me several days longer than I'd expected due to my focus on making the remotes work as I'd wanted, not all was lost. At the very least, I gained some important insights for the road ahead!
