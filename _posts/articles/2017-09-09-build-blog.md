---
layout: post
title: "Brief Sequence to Build Blog"
categories: "article"
excerpt: "Commands"
tags: ["Ruby", "jekyll" ]
---

`## get the source code`

git clone <repo> 

`## if there is error in "bundle install", recompile hitimes`

sudo apt-get install ruby ruby-dev
sudo gem install hitimes -v '1.2.2' --platform ruby

`##proceed with the install`

bundle install
bundle exec jekyll serve

