---
layout: post
title: Fix gems/plugins version issue in GitPage
categories: [GitPage,Ruby]
tags: [GitPage,Ruby]
fullview: true
---

I got a security alert of my GitPage (this one) today. It was because the dependencies of my site were out of date which might cause some security issues.<br><br>
I'm not sure if my workaround is the best practice, but it worked so I just put it here. <br><br>
First I installed [Bundler](https://bundler.io/) in my computer, then I `cd` into the directory of my gitpage and run `bundle update`. I encountered some issues of lacking header files when updating dependencies so that make sure you have ruby-dev (or ruby-devel, depend on your OS) installed. <br><br>
Running `bundle exec jekyll serve` to test your website before pushing it to GitHub is highly recommended. I encountered some issues which nested in `bundle` directory where I installed ruby plugins. Those "issues" are usually date format in example project or symbolic links issues of plugins themselves which don't affect my project so we can simply add `exclude: [vendor]` (vendor here can be changed, depends on your directory which contains the bundle and plugins files) in _config.yml. 
