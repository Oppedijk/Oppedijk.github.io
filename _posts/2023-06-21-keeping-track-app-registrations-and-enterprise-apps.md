---
layout: post
title: "Keeping track of App Registrations and Enterprise Apps"
excerpt_separator: <!--more-->
---
In this post we will have a look at managing Enterprise Apps and App Registrations
What do we want to add:
 - Add Owner of the app
 - Add description about the app
<!--more-->

Work in progress:

### AZ CLI

(from CI/CD, add description automatically) 
az ad app update --id bc74bd97-2142-4ef7-9752-41beg840379a --set notes="erik"

Notes haved a maximum of 1024 characters (including cr\LF)



