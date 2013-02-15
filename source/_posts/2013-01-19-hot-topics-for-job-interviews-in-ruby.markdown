---
layout: post
title: "hot topics for job interviews in ruby"
date: 2013-01-19 15:44
comments: true
categories: [Ruby, Job Interviews, Bay Area]
---

Been looking for jobs as a web developer / rails developer / ruby developer / software engineer?  So have I for about 3 montsh now.  I've worked previously for a few years using ruby as well as rails from time to time.  You'd think that ruby and rails were the same thing from all the hype. Here's my list of things to know for a ruby tinted interview....

1. Know about the iterators: each, inject, map
  [1,2].inject(2){|k,v| v + k}
   => 5
   [1,2].map{|k,v|  k**2 }
   => [1,4]

2. What are the four types of closures in ruby?
   This one gets asked differently... AKA - what is the difference between proc, lamda, and a method?  they are all closures.  The fourth closure is a block.

3. What is method missing?

    class Foo

    def method_missing(method,*args)
      puts "you tried to call #{method} but it doesnt exist"
      puts "Why not define a new method called #{method} that accepts #{args}"
    end

    end

    Foo.new.missing_method("some parameter that ", "is missing")
    =>
    you tried to call missing_method but it doesnt exist
    Why not define a new method called missing_method that accepts ["some parameter that ", "is missing"]


4.  Can you tell me how to do inheritance in basic ruby?
  Also asked as : what is the difference between include and extend?

  You can include or extend a module to inherit methods.

5.  What are some assumptions active record makes?

  It assumes that you have a primary key of one...


