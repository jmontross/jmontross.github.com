---
layout: post
title: "How to implement captcha on 100 php contact pages in one script using ruby"
date: 2013-12-12 11:56
comments: true
categories:
---

The assignment:  Can you update 100+ websites contact pages with a captcha form so I stopped getting spammed?

The solution:  Create a new contact.php file with captcha implemented, check to see if the sites contact.php matches the first one I looked at, if it is the same, swap the new contact file for the old and keep a running list of how many you've updated.

This is a ruby script that implements the solution above in real code.  The files described in the file are provided at the end in a bit of a redacted form.

    require 'fileutils'

    f
    ilures = []
    successes = []

    Dir.entries('/home3/northas0/public_html').each do |directory|

      if Dir.exists?("/home3/northas0/public_html/#{directory}") && Dir.chdir
      "/home3/northas0/public_html/#{directory}")
        if system('diff contact.php ../library/contact_old.php')
        then
          FileUtils.cp('../library/contact.php',"/home3/northas0/public_html/#{directory}/contact.php")
          puts "contact.php replaced at /home3/northas0/public_html/#{directory}/contact.php"
          successes << directory
        else
          puts "failed to replace contact.php differences in /home3/northas0/public_html/#{directory}"
          failures << directory
        end
      end

      end
    puts "there were #{successes.length} files replaced"
    puts "failed directories #{failures}"
    puts "success at #{successes}"


[Here's the scripts](https://gist.github.com/jmontross/bc382af5340ea3eb4e1f)

The key is that I reused the contact.php logic and implemented using a recaptcha account for more than one domain. To do this just check the box when creating your [recaptcha account](https://developers.google.com/recaptcha/intro)

You can even [customize recaptcha](https://developers.google.com/recaptcha/docs/customization)

Thanks.  Hope it was helpful.