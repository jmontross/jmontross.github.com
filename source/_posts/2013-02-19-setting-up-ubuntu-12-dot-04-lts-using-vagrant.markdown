---
layout: post
title: "setting up ubuntu 12.04 LTS using vagrant"
date: 2013-02-19 11:06
comments: true
categories:
---

So I have some code that runs nginx and node, but I'd like to do this in a vm because my mac doesnt quite run the exact same as the production ubuntu.  Since development is production I'd like replicable steps to make a VM on a local machine and run our nginx/node app.  There are two ways to go about this once we decide to use vagrant.

1.  Use standard boxes already built from isos and publicly avaiable in .box format
2.  Make our own .box with custom settings from an iso at official http://www.ubuntu.com/download/server

We can use use an out of the box available VM from http://www.vagrantbox.es/
Let's go down this route and save the custom ubuntu precise for another day.

     vagrant box add ubuntu-12-04-daily http://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-vagrant-i386-disk1.box

You'll have to wait 10 to 20 minutes if you've got reasonably fast internet
vagrant init
modify vagrant file to have

    config.vm.box = "ubuntu-12-04-daily"
    config.vm.forward_port 443, 6666
    config.vm.forward_port 80, 8080

Now save and close that vagrant file
If you are interested in the port forwarding read http://docs.vagrantup.com/v1/docs/config/vm/forward_port.html
Otherwise, back to the command line to get inside this new VM.

    vagrant up
    vagrant ssh

now you are inside the virtual machine and should see vagrant@vagrant-ubuntu-precise-32

    sudo apt-get install nginx
    sudo ngnix

Then browse to http://0.0.0.0:8080/ and we see Welcome to Nginx!

Now I want to have node and mongo...  I use this code

    wget https://raw.github.com/punkave/stagecoach/master/sc-proxy/install-node-and-mongo-on-ubuntu.bash
    sudo bash install-node-and-mongo-on-ubuntu.bash

So I did this because I wouldnt mind having the latest... but I'd also like to build from the trustworthy source.  http://www.ubuntu.com/download/server

I got a file called ubuntu-12.04.2-server-amd64.iso

    mv -f ~/Downloads/ubuntu-12.04.2-server-amd64.iso ~/code/vms

Then I add a new vm using virtualbox and configure it to use this iso as a cd drive.  I created a vm called ubuntu12.04, and this is the same name I gave it on the network.  I gave it a user called vagrant and a password called password... When I boot up it works!


I follow some steps from here http://pyfunc.blogspot.com/2011/11/creating-base-box-from-scratch-for.html

    sudo apt-get install build-essential
    sudo apt-get install ruby1.8
    curl -L https://get.rvm.io | bash -s stable --ruby
    source /hom/vagrant/.rvm/scripts/rvm


    got a key from here ... https://raw.github.com/mitchellh/vagrant/master/keys/vagrant.pub


I realize this file is bad... this is the good one . 4 keys -

    http://docs.vagrantup.com/v1/docs/base_boxes.html

    1 VirtualBox Guest Additions for shared folders, port forwarding, etc.
    2 SSH with key-based auth support for the vagrant user
    3 Ruby & RubyGems to install Chef and Puppet
    4 Chef and Puppet for provisioning support

ADDED /opt/company-name/

symlinked code to there

created shared folder to this area.  vbox, settngs, shared folders

Wow - got ahead of myself. need item 1.  items 2-4 are not so hard.

    1. Run your Ubuntu Server Virtual Machine

    2. From the Devices menu select Install Guest Additions.

    3. Now you are supposed to have the guest additions .iso image in your virtual     machine’s CD.

    4. Now you might find a problem that is the cdrom is not automatically mounted, so you need to     do one more step before starting the installation:

        $ sudo mount /dev/cdrom /media/cdrom
        block device /dev/sr0 is write-protected, mounting read-only

    That will do the trick.

    5. Move to the cdrom directory

        $ cd /media/cdrom/
    6. When listing the contents you should see something like this:

        $ ls -la
        total 37429
        dr-xr-xr-x 4 root root     2048 2011-06-24 15:45 .
        drwxr-xr-x 3 root root     4096 2011-07-17 16:20 ..
        dr-xr-xr-x 3 root root     2048 2011-06-24 15:45 32Bit
        dr-xr-xr-x 2 root root     2048 2011-06-24 15:45 64Bit
        -r-xr-xr-x 1 root root      647 2011-01-19 14:42 AUTORUN.INF
        -r-xr-xr-x 1 root root     6966 2011-06-24 15:40 autorun.sh
        -r-xr-xr-x 1 root root     5523 2011-06-24 15:40 runasroot.sh
        -r-xr-xr-x 1 root root  7863758 2011-06-24 15:43 VBoxLinuxAdditions.run
        -r-xr-xr-x 1 root root 14665216 2011-06-24 16:44 VBoxSolarisAdditions.pkg
        -r-xr-xr-x 1 root root  9294616 2011-06-24 15:31 VBoxWindowsAdditions-amd64.exe
        -r-xr-xr-x 1 root root   278832 2011-06-24 15:24 VBoxWindowsAdditions.exe
        -r-xr-xr-x 1 root root  6199880 2011-06-24 15:25 VBoxWindowsAdditions-x86.exe
    7. Now it is time to run the installation

        $ sudo ./VBoxLinuxAdditions.run

http://tadabborat.tumblr.com/post/7881270430/virtualbox-shared-folders-on-ubuntu-server-64bit

K - on to second item...

    sudo apt-get install ssh

    rm -r "$(gem env gemdir)"/doc/*.

sources
http://pyfunc.blogspot.com/2011/11/creating-base-box-from-scratch-for.html
http://seletz.github.com/blog/2012/01/17/creating-vagrant-base-boxes-with-veewee/
http://chrisyallop.com/2012/04/customising-a-vagrant-box-with-veewee/
https://github.com/jedi4ever/veewee/commit/31133bd14b3562aa4e9479c6b964450562838602
https://www.digitalocean.com/community/articles/how-to-install-ruby-on-rails-on-ubuntu-12-04-lts-precise-pangolin-with-rvm
http://docs.rubygems.org/read/chapter/3

taking this a step further one day ...
https://github.com/opscode/bento

