---
layout: post
title: "setting up something secret on heroku"
date: 2013-02-13 14:23
comments: true
categories:
---

I was wanting to run stripe on my new site www.karmagrove.com

I put the fake keys into my config file like a bad developer/ops person, which is of course a security problem because it's an open source app.

So I add some new yaml file called strip.yaml to my config

    api_key: ENV['stripe_api_key']
    public_key: ENV['stripe_public_key']

I create a new file called setup_heroku_keys.sh and add it to my git ignore.

    touch setup_heroku_keys.sh && echo "setup_heroku_keys.sh" >> .gitignore

Then I add the following lines to my setup_heroku_keys.sh file

    heroku config:add STRIPE_API_KEY=sk_live_MY_SECRET
    heroku config:add STRIPE_PUBLIC_KEY=pk_live_MY_PUBLISHABLE_KEY

    bash setup_heroku_keys.sh

    =>
    Setting config vars and restarting karma-grove... done, v16
    STRIPE_API_KEY: sk_live_HIDDEN
    Setting config vars and restarting karma-grove... done, v17
    STRIPE_PUBLIC_KEY: pk_live_HIDDEN

I got this inspiration from the helpful heroku docs...
https://devcenter.heroku.com/articles/config-vars



