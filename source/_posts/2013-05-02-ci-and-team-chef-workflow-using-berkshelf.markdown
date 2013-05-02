---
layout: post
title: "CI and Team Chef Workflow Using Berkshelf"
date: 2013-05-02 16:57
comments: true
categories: OpsChef, CI, Berkshelf
---

Inspiration
-----------

In the past two weeks I have been to both ChefConf and DevOpsDays Austin.  Both of these events
were full of genuinely smart, geeky, and accessable folks who were all a pleasure to talk to.

One of the hot topics on my agenda is building a workflow for team Chef development that
allows for continuous integration to be performed on cookbooks, and prevents folks from stepping
on each other's toes on a shared Chef server.  A key component of this workflow is Berkshelf.

In addition to the conferences, I have taken a LOT of inspiration from several key episodes of the FoodFightShow. 

This post is my take on of the ideas of others.  Without the great conversation and tooling coming from the community, I'd have nothing.  :)

Key Points and Assumptions
--------------------------
- Developers must be able to collaborate on a single Chef server without squashing each others changes.
- Cookbooks must be treated like independent software projects with a name, dependencies, a version, a git repository, a CI job, unit tests and functional tests.
- Cookbooks uploaded to a Chef server must be treated as versioned immutable artifacts.  

Credits
-------

* [Berkshelf] - Berkshelf.com has a great overview of how to get started.
* [Jamie@ChefConf] - Jamie Winsor gave an awesome talk about 'The Berkshelf Way' at ChefConf 2013.
* [FoodFightShow] - Jamie Winsor gave an awesome talk about 'The Berkshelf Way' at ChefConf 2013.

*Berkshelf FTW.*

  [Berkshelf]: http://berkshelf.com/
  [Jamie@ChefConf]: http://www.youtube.com/watch?v=hYt0E84kYUI
  [FoodFightShow]: http://www.foodfightshow.org
  
  

    