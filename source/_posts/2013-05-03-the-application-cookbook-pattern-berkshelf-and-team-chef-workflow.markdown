---
layout: post
title: "The Application Cookbook Pattern, Berkshelf, and Team Chef Workflow"
date: 2013-05-03 21:48
comments: true
categories: [DevOps,ChefConf,OpsChef]
published: true
---

Berkshelf
---------

I am a fan of "The Berkshelf Way".

If you aren't familiar with Berkshelf or the Application Cookbook pattern, do yourself a favor and spend some time on the following links...

<!--more-->

* [Berkshelf] - The official Berkshelf docs.
* [Jamie@ChefConf] - Jamie Winsor's "The Berkshelf Way" talk from [ChefConf] 2013.
* [GangnamStyle] - Bryan Berry's "How to Write Reusable Chef Cookbooks, Gangnam Style" blog post.
* [FoodFightShow24] - Jamie Winsor and Michael Ivey from Riot Games on the FoodFightShow.  Why they wrote Berkshelf, how they develop Cookbooks, why they don't like Roles, and more.
* [FoodFightShow32] - Etsy and Riot Games both talking about their unique approaches to Team Chef Development.
* [GuideToAuthoringCookbooks] - A tutorial that will demonstrate the awesomeness of the workflow that the [Berkshelf]/[Vagrant] combo provides.

*The authors of the above content deserve nearly full credit for the rest of this blog post, and a number of ideas I have for future posts.*

I have had great success using [Berkshelf]/[Vagrant] to rapidly iterate on a Cookbook and get quick feedback on changes.  And now with Vagrant 1.1's provider plugin interface, this workflow does not limit you to just Virtualbox.  There are already provider plugins for AWS and Rackspace among others.  Hurrah!

This works very well for initial development and quick fixes, but when you need to have multiple teams collaborating on a shared set of cookbooks, there are some additional factors to take in to consideration.

Team Chef Development
---------------------

The next step is figuring out how to manage Cookbook versioning in a multi-user development environment, and ensuring that teams aren't repeating each others work or stepping on each other's toes.  Cookbooks must be shared between teams, and these shared Cookbooks must be well-tested given the impact is potentially much wider than just a single team.  This is a bit trickier...

Here are a few ground rules that could be the basis of a sound Team Chef Development Workflow:

* Versioning of internal Cookbooks should be managed by the build server, and ONLY the build server.
* Application Cookbook verions are pinned at the Environment level.
* Library Cookbook versions are pinned in the metadata.rb of the Application Cookbook that includes them.
* Only Application Cookbooks get assigned to Nodes.  Not Library Cookbooks, or Roles.  (Stick with me for a second...)
* Library Cookbooks should change their behavior based upon Attributes.
* Defaults for Attributes are provided in the Library Cookbook attributes/default.rb.  Default Attributes are only over-ridden in two places: the metadata.rb of the Application Cookbook, and the Environment.   This makes troubleshooting much easier.  Setting Attributes in Roles seems unnecessary and just adds an extra place to look (a place that is not versioned).
* Search by Tags or a custom Attribute to locate Nodes, not by run_list entry or Role.  Provide functions to call these searches in a Library Cookbook, so the location mechanism is easily swapped out or refactored down the road.  (Or abandon search all together and start using a service registry like ZooKeeper, but that's another blog post).
* Community Cookbooks pulled in as dependencies by Berkshelf should not be forked internally, unless it is unavoidable.  If you must fork perhaps due to an unfixed bug, strive to update the Cookbook, make it more flexible to meet your needs, add some tests, and send in a pull request.  This way the community benefits from the work, and you will continue to gain the benefits of future community-added features without the burden of maintaining and updating a fork.

Why Roles Are An Issue
----------------------
This is a controversial subject, but I have a problematic use case that we have already been bitten by, and I cannot solve easily with Roles.  

* Teams A, and B are both working on the Chef server.  Both teams are working on the web_server Role.  
* Team B needs to run a bleeding edge version of the web stack which requires an additional Recipe in the web_server run_list.  
* Team A's stuff explodes because Roles are global variables and the new Recipe isn't in their Cookbook.  

I know you can setup Environment-specific run_lists, but we routinely have 15+ dev/test environments up, and don't see this number falling any time soon.  This is clearly not a maintainable solution.  

At the end of the day, Roles only do 3 things:

1. Manipulate the run_list.
2. Set Attributes.
3. Tag a node with a friendly name.

If you use an Application Cookbook to accomplish #1 and #2, then suddenly your Role is versioned and can be pinned differently per Environment on your Chef server.  To handle #3, change your Chef searches to look for a Node Tag or custom Attribute instead of a Role, and gain a little bit of flexibility in the process.  Ever wanted to verify a new web server by hand before you brought it in to the load balancer pool?  Don't Tag it or set the custom Attribute until you've validated the Node, instead of having the Role immediately match a search.

Thoughts on any of this?  Anything I should go in to more detail on?

I'm planning on some future posts to go in to more detail on some of these areas...  Particularly CI and testing!

  [ChefConf]: http://chefconf.opscode.com/
  [Berkshelf]: http://berkshelf.com/ 
  [Vagrant]: http://vagrantup.com/ 
  [Jamie@ChefConf]: http://www.youtube.com/watch?v=hYt0E84kYUI
  [GangnamStyle]: http://devopsanywhere.blogspot.com/2012/11/how-to-write-reusable-chef-cookbooks.html
  [FoodFightShow]: http://foodfightshow.org
  [FoodFightShow24]: http://foodfight.libsyn.com/episode-24-jamie-winsor-and-michael-ivey-on-berkshelf
  [FoodFightShow32]: http://foodfight.libsyn.com/episode-32-there-s-a-spork-in-my-berkshelf-talkin-bout-workflow
  [GuideToAuthoringCookbooks]: http://vialstudios.com/guide-authoring-cookbooks.html