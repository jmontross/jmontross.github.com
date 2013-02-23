---
layout: post
title: "pairing with tmux and rspec on aws ec2 instance"
date: 2013-02-21 13:42
comments: true
categories:
---

I wanted to pair with my friend who sat down next to me... each of us on our own laptop, but none of the gear that makes it easy for us to actually pair in the sense of two keyboards one screen.  I luckily have already burned through my AWS free tier and keep around an ubuntu server for playing/learning.  I ask my friend if he has ever used vim?  He says no.  Is he interested in pair programming on a server using vim and tmux, and he says sure. Thanks Dan for being such a good sport.

I ssh into my machine

    useradd dan
    password: secret

    sudo vi /etc/ssh/sshd_config
    :s/PasswordAuthentication no/PasswordAuthentication yes/
    :wq

    sudo /etc/init.d/sshd reload

sweet.  now dan can login.  I want him to be able to share a vim window where we can view a test suite and the code split panel.  How do I do that I wonder?

     ## specify the name of your tmux socket with -S when creating it
     tmux -S /tmp/pair
     # chmod to allow other users to access it
     chmod 777 /tmp/pair

Now I've got my tmux going, dan is in terminal, and I want to share our screen.

     # now the other user can connect with
     tmux -S /tmp/pair attach

Awesome.  I cd into ~/home/dan and make a directory where we can do the fizzbuzz problem using tdd.  This next step assumes you've got rubygems on your system and

     mkdir fizzbuzz
     cd fizzuzz
     mkdir spec
     touch spec/fizzbuzz_spec.rb
     touch fizzbuzz.rb

     echo "rspec" >> Gemfile
     bundle
     => installing rspec....
     rspec

0 tests and time to get started...  Where do we start?

     vi .
     ctrl+wv

now we've got two windows each with directory open like so ...

Imagine there's an image here...

I make a test in the fizzbuzz_spec.rb file

    require 'rspec'
    require './fizzbuzz'

    describe FizzBuzz do
      it "should instantiate" do
        FizzBuzz.new.class == FizzBuzz
      end
    end

We need to save it...

    :wq

we need to run tests

    :! rspec

Its a nice workflow... it works....

sources: http://readystate4.com/2011/01/02/sharing-remote-terminal-session-between-two-users-with-tmux/

http://stackoverflow.com/questions/8339912/allowing-users-to-ssh-to-an-ec2-ubuntu-instance

http://pivotallabs.com/how-we-use-tmux-for-remote-pair-programming/