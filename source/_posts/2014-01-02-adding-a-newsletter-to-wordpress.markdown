---
layout: post
title: "adding a newsletter to wordpress"
date: 2014-01-02 18:59
comments: true
categories:
---

I found some sweet [newsletter software](http://wordpress.org/plugins/newsletter/) and added it to my wordpress.

Downloaded the file from there..

    scp /Users/josh/Downloads/newsletter.zip root@192.81.131.92:/var/www/josh/wp-content/plugins

    ssh root@192.81.131.92
    cd /var/www/josh/wp-content/plugins
    unzip newsletter

Then I go into my wordpress panel and activate the plugin.  Super easy!


The next problem is that when I subscribe it doenst really send the email.

    sudo apt-get install sendmail

Now it sends email but it ends up in the spam folder... How do I fix this one?

Looks like I'll get that one next time... in the mean time, feel free to read my blog at [joshuamontross.com](http://www.joshuamontross.com) and subscribe to the newsletter :)  Be sure to check your spam folder for now...
