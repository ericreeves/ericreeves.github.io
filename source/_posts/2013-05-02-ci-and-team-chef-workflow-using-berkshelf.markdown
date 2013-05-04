---
layout: post
title: "CI and Team Chef Workflow Using Berkshelf"
date: 2013-05-02 16:57
comments: true
categories: [OpsChef, CI, Berkshelf]
published: false
---

One of the hot topics on my agenda lately is building a workflow for team Chef development that prevents folks from
stepping on each other's toes on a shared Chef server, and preventing changes to shared LWRP/libraries from blowing up
all the things.  Key elements of this workflow are cookbook re-use and shareability within the organization, continuous
integration for each cookbook, and a mechanism for handling cookbook versioning.

<!--more-->


This post isn't near finished, and the blog hasn't been publicized yet.  If you found this, you are some kind of Internet wizard.  Please don't share it until this text is gone.  Thanks!  :)


Inspiration
-----------

In the past two weeks I have been to both [ChefConf] and [DevOpsDaysAustin].  Both of these events were full of genuinely
smart, geeky, and accessable folks who were all a pleasure to talk to. In addition to the conferences, I have taken a
lot of inspiration from several key episodes of the [FoodFightShow].

This post is a collection of my thoughts and opinions based on the ideas of others.  Without the great conversation and tooling coming from the community, I'd have nothing.  So if you like what you see, make sure you thank the folks linked in the Credits.  :)


Key Points and Assumptions
--------------------------
- Developers must be able to collaborate on a single Chef server without squashing each others changes.
- Cookbooks must be treated like independent software projects with a name, dependencies, a version, a git repository, a CI job, unit tests and functional tests.
- Cookbooks uploaded to a Chef server must be treated as versioned immutable artifacts.  

Phases of Cookbook Development
------------------------------

* Create a new cookbook using Berkshelf, or clone an existing cookbook repo.
* Write / modify the cookbook to taste.
* Verify validity of metadata.rb.  Ensure dependencies are set correctly.
* Test locally with 'vagrant up' for the initial VM launch, and 'vagrant provision' any time the cookbook is modified and needs to be re-converged on the node.
* Once satisfied, commit the changes to Git.  Existing cookbooks will have CI jobs setup which will run Foodcritic, ChefSpec, Minitest Handler.  If tests are successful, bump the metadata.rb version, tag the release in Git, and upload/freeze to the 'Repository Chef Server'.

... but wait, what the hell is a repository Chef server?

Berkshelf allows you to specify a Chef server as a source for resolving cookbook dependencies.  This means you can use a Chef server as a local repository for tested, usable cookbooks.  It also means you can let Jenkins/CI be the source of authority for managing cookbook versions, so you don't have multiple people attempting to bump versions and colliding.  Win.

Important to note is that when the CI server uploads to Chef, it freezes the cookbooks.  This way, you will know with near certainty that the Hooha cookbook v1.0.0 is immutable, so if you use it as a dependency in your cookbook and pin the version, it will always produce the same results. (unless somebody uploads over it with a --force, in which case they should be publically shamed and have their Chef keys revoked until they learn how to play nice...)

Cookbook Shareability
---------------------

In order for multiple teams to be able to effectively work together on a set of Chef cookbooks, it is particularly important that cookbooks be made as reusable as possible, and that silos aren't repeating work.  A common set of organizational 'base' cookbooks, Berkshelf, and the 'Application Cookbook' pattern is the way to make this happen.  If you aren't familiar with this pattern, spend a few minutes reading the [Berkshelf] docs, 45 minutes watching Jamie Winsor's "The Berkshelf Way" talk from [ChefConf] 2013, or read Bryan Berry's [GangnamStyle] blog post. 

There are a few important points:

* Library cookbooks should change their behavior based upon configured attributes.
** You should never apply the 'apache2' cookbook directly to a node, or even a role.  You should use 'include_recipe' in an Application Cookbook, and override all of the attributes you need to configure in its metadata.rb.
* Community cookbooks pulled in as dependencies by Berkshelf should not be forked internally, unless it is unavoidable.  If the need to fork is a must, strive to update the cookbook, make it more flexible to meet your needs, add some tests, and send in a pull request.  This way the community benefits from the work, and you will continue to gain the benefits of future community-added features without a bunch of potentially ugly merging.
* Versioning of organizational cookbooks should be managed by the build server, and ONLY the build server.

Credits (in no particular order)
--------------------------------

* [Berkshelf] - Berkshelf.com has a great overview of how to get started.
* [Jamie@ChefConf] - Jamie Winsor gave an awesome talk about 'The Berkshelf Way' at ChefConf 2013.
* [GangnamStyle] - Bryan Berry's great blog post about the the application/wrapper cookbook pattern.
* [FoodFightShow] - If you use Chef, you should listen to this show.  Because of reasons.
* [ChefConf] - Everybody.
* [DevOpsDaysAustin] - Specifically the folks who participated in the Berkshelf open space.



*Berkshelf FTW!*

  [Berkshelf]: http://berkshelf.com/
  [Jamie@ChefConf]: http://www.youtube.com/watch?v=hYt0E84kYUI
  [GangnamStyle]: http://devopsanywhere.blogspot.com/2012/11/how-to-write-reusable-chef-cookbooks.html
  [FoodFightShow]: http://www.foodfightshow.org
  [ChefConf]: http://chefconf.opscode.com/
  [DevOpsDaysAustin]: http://devopsdays.org/events/2013-austin/
  
  

    
