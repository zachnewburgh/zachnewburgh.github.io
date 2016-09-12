---
layout: post
title:  "I Gotta FeeliNg"
date:   2016-09-12 19:30:51 +0000
---

### So you've learned AngularJS, but do you know how to integrate it with Rails?

It's been just over six months since I started the Flatiron School's self-driven Full Stack Web Development program – and what a ride it's been!

When explaining my journey with Learn.co, I've seen my friends widen their eyes with both excitement and wonder. How someone with customer-facing, operations, and management experience could dive deep into code seemed to amaze them in ways that I hadn't expected.

So many of them have claimed that programming is extraordinarily difficult, that it's elusive, and that one needs to have the mind for it. But I'm not sure if that's true.

For me, the challenging part of Learn.co wasn't the material itself. Sure, it took plenty of time, effort, and patience to learn new concepts – whether object-oriented programming, databasing, or front-ending – but the features themselves were more like puzzles whose pieces were coded in a logical language that could be learned.

Instead, the most difficult climb was putting it all together. Figuring out how to iterate over a hash of arrays, implement Regex for validation, or configure the database weren't so hard, but laying the foundation of a Gem, setting up a Sinatra, app, scaffolding in Rails, and integrating JQuery from scratch proved to be the biggest issues.

This was no different with creating a Rails app whose foundation depended on AngularJS. I spent days searching the web for helpful guides – all for naught. Thankfully, when I finally reached out to the staff team at Learn.co, I was directed to Luke Ghenco's fantastically helpful **[four-part series](https://medium.com/@lukeghenco/create-an-angular-js-app-with-a-restful-rails-api-pt-1-58121eb6e4f9#.ozl4ni4lq)**.

Tutorials that instruct how to integrate AngularJS with Rails seem to be few and far in between. So, this blog post is dedicated to adding another:

**Step 1: Generate Rails App**

Open up your Terminal and type:

```
$ rails new [app_name] --skip-javascript
```

**Step 2: Build the Model**

```
$ rails g model [model_name] [attribute]:[attribute_type]
```

**Step 3: Build the Model**

