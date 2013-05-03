---
layout: post
title: "CI and Team Chef Workflow Using Berkshelf"
date: 2013-05-02 16:57
comments: true
categories: [OpsChef, CI, Berkshelf]
---

One of the hot topics on my agenda lately is building a workflow for team Chef development that prevents folks from
stepping on each other's toes on a shared Chef server, and preventing changes to shared LWRP/libraries from blowing up
all the things.  Key elements of this workflow are cookbook re-use and shareability within the organization, continuous
integration for each cookbook, and a mechanism for handling cookbook versioning.

<!--more-->

Inspiration
-----------

In the past two weeks I have been to both ChefConf and DevOpsDays Austin.  Both of these events were full of genuinely
smart, geeky, and accessable folks who were all a pleasure to talk to. In addition to the conferences, I have taken a
LOT of inspiration from several key episodes of the FoodFightShow.

This post is my take on of the ideas of others.  Without the great conversation and tooling coming from the community,
I'd have nothing.  So if you like what you see, make sure you thank the folks linked in the Credits.  :)

Key Points and Assumptions
--------------------------
- Developers must be able to collaborate on a single Chef server without squashing each others changes.
- Cookbooks must be treated like independent software projects with a name, dependencies, a version, a git repository, a CI job, unit tests and functional tests.
- Cookbooks uploaded to a Chef server must be treated as versioned immutable artifacts.  

Credits
-------

* [Berkshelf] - Berkshelf.com has a great overview of how to get started.
* [Jamie@ChefConf] - Jamie Winsor gave an awesome talk about 'The Berkshelf Way' at ChefConf 2013.
* [GangnamStyle] - Bryan Berry's great blog post about the the application/wrapper cookbook pattern.
* [FoodFightShow] - If you use Chef, you should listen to this show.  Because of reasons.

This post isn't near finished, and the blog hasn't been publicized yet.  If you found this, you are some kind of Internet wizard.

*Berkshelf FTW!*

  [Berkshelf]: http://berkshelf.com/
  [Jamie@ChefConf]: http://www.youtube.com/watch?v=hYt0E84kYUI
  [GangnamStyle]: http://devopsanywhere.blogspot.com/2012/11/how-to-write-reusable-chef-cookbooks.html
  [FoodFightShow]: http://www.foodfightshow.org
  
  

    
