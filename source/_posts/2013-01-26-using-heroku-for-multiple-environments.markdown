---
layout: post
title: "Using heroku for multiple environments"
date: 2013-01-26 07:33
comments: true
categories:
---

I have been working on a simple rails app @ lessonOverFlow.com with a couple friends and I started noticing that some deploys to the website would break the prod site because of some difference in my local mac environment and the heroku servers.  Even though we have no users, I'd rather figure out these speedbumps on a development or staging server.  Let's call it development.

First thing is to point my DNS to heroku's servers with a CNAME record

With namecheap it was as simple as adding in this reference...

Login... click "all host records"

add a subdomain setting for dev that points to #{dev-your-heroku-appname-here}.herokuapp.com.

The period at the end counts.  1800 for TTL.

make sure of course that you also do a heroku create dev-your-heroku-appname-here

After this point the guys at stack overflow have already answered it pretty well.

  git remote add staging staging-your-app.heroku.git
  git push staging staging:master

http://stackoverflow.com/questions/7713042/deploying-2-different-heroku-apps-with-same-git-repository

Now what does the staging:master mean in git push command above and in the answer on SO?