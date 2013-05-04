---
layout: post
title: "The Application Cookbook Pattern and Berkshelf"
date: 2013-05-03 21:48
comments: true
categories: [DevOps,ChefConf,OpsChef]
published: false
---

I am a fan of "The Berkshelf Way".

If you aren't familiar with Berkshelf or the Application Cookbook pattern, do yourself a favor and spend some time on the following links...  This is the basis for a number of ideas I have for future posts.

<!--more-->

* [Berkshelf] - The official Berkshelf docs.
* [Jamie@ChefConf] - Jamie Winsor's "The Berkshelf Way" talk from [ChefConf] 2013.
* [GangnamStyle] - Bryan Berry's "How to Write Reusable Chef Cookbooks, Gangnam Style" blog post
* [GuideToAuthoringCookbooks] - A tutorial that will demonstrate the awesomeness of the workflow that the [Berkshelf]/[Vagrant] combo provides.

In order for multiple teams to be able to effectively work together on a set of Chef cookbooks, it is particularly important that cookbooks be made as reusable as possible, and that silos aren't repeating work.  A common set of organizational 'base' cookbooks, Berkshelf, and the 'Application Cookbook' pattern is the way to make this happen.  

There are a few important points:

* Only Application Cookbooks get assigned to nodes.  Not Library Cookbooks, or complex roles.  (Stick with me for a second...)
* Library cookbooks should change their behavior based upon configured attributes.
* You should never apply the 'apache2' cookbook directly to a node, or even a role.  You should use 'include_recipe' in an Application Cookbook, and override all of the attributes you need to adapt to your environment in its metadata.rb.
* Search by Tags to locate nodes, not by run_list entry or role.  This is a much more flexible solution, and allows you to do things like only add your web server to the load balancer once you have verified it and set the tag.  (Or abandon search all-together and start using a service registry like ZooKeeper, but that's another blog post).
* Community cookbooks pulled in as dependencies by Berkshelf should not be forked internally, unless it is unavoidable.  If the need to fork is a must, strive to update the cookbook, make it more flexible to meet your needs, add some tests, and send in a pull request.  This way the community benefits from the work, and you will continue to gain the benefits of future community-added features without a bunch of potentially ugly merging.
* Versioning of organizational cookbooks should be managed by the build server, and ONLY the build server.


  [ChefConf]: http://chefconf.opscode.com/
  [Berkshelf]: http://berkshelf.com/ 
  [Vagrant]: http://vagrantup.com/ 
  [Jamie@ChefConf]: http://www.youtube.com/watch?v=hYt0E84kYUI
  [GangnamStyle]: http://devopsanywhere.blogspot.com/2012/11/how-to-write-reusable-chef-cookbooks.html
  [FoodFightShow]: http://foodfightshow.org
  [GuideToAuthoringCookbooks]: http://vialstudios.com/guide-authoring-cookbooks.html