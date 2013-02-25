---
layout: post
title: "deploying with chef on a new mac"
date: 2013-02-22 14:29
comments: true
categories:
---

I really dislike setting up machines more than once.

This is my up to date steps on how I set up my new macintosh machines.

Whether I should have gone for anti-glare or retina display is hard enough... should I decide to switch the last thing I want to be worrying about is set up time for the new machine.

With the following approach, the biggest bottleneck is bandwidth.

https://gist.github.com/jmontross/5015950

If you encounter an error - which I often do - just delete the offending recipe unless you really want it.  I built the file using

http://www.solowizard.com/

Most recently it got hung up on activemq - which i subsequently removed because I don't really need that project.  Then it got hung up on Java... which I was able to just click the package to install, and then

     bash set_up_my_machine.sh

Then it gets hung up on librsvg

    vi set_up_my_machine.sh
    /librsvg
    dd
    :ew

    bash set_up_my_machine.sh


Then it got hung up on the use of a password for mysql... so I ripped out the usage of a password at all.. I prefer my mysql unprotected.  It's getting close...

That didn't work.

I removed the steps that were executing commands on mysql.  I dont care about mysql timezones anyways.

Got hung up on skype. Rerun.

Works. I pushed the code to my branch at https://github.com/jmontross/pivotal_workstation


    1. installing command line tools for osx (download x-code from app store, open x-code,     preferences -> downloads > command line tools)
    2. mkdir ~/cookbooks; cd ~/cookbooks; git clone https://github.com/opscode-cookbooks/dmg;     git clone git://github.com/jmontross/pivotal_workstation.git
    3. gem install soloist
    4. bash pivotal_workstation/set_up_my_machine.sh
    5. wait a while




