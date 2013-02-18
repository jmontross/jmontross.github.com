---
layout: post
title: "new mac set up with chef"
date: 2013-02-15 18:31
comments: true
categories:
---
why? Because I love TDD, DRY, and rubymine shortcuts.  Pivotal did a good thing with this soloist gem, and I enjoy making use of great ideas that solve problems I have experienced.  As much fun as it is to set up my new laptop with memcached, mysql, redis, mongo, and insert_dependency_here I've done it enough times to realize it is not really a valuable learning experience one need repeat.

Problem is.... it broke?  Why?  I added some of my own custom recipes and the newest mac has issues with gcc and ruby when I wanted to use 1.8.7 as a rvm version for an older project.  Either way, This is what my little script I use looks like. I run it with bash.  I made it using http://www.solowizard.com/ and the code at https://github.com/pivotal/pivotal_workstation

To get chef-solo working these steps were required
had to clone pivotal_workstation and dmg into ~/cookbooks, get commands line tools from mac (http://stackoverflow.com/questions/9329243/xcode-4-4-command-line-tools), gem install soloist.  Then you are able to just run the command bash setup_my_machine.... make sure you have the recipes.... some of the ones in this list below are custom from my cookbooks repo (https://github.com/jmontross/pivotal_workstation)...


    cat ~/cookbooks/setup_my_machine.sh
    #!/bin/bash
    # This shell script was created at solowizard.com
    #
    # Usage:
    #   Running the script remotely:
    #     bash < <(curl -s http://www.solowizard.com/soloist_scripts/frsp9k.sh )
    #   Running the script if you have downloaded it:
    #     ./frsp9k.sh
    #
    # http://github.com/tommyh/solo_wizard
    # (c) 2012, Tom Hallett
    # This script may be freely distributed under the MIT license.

    pushd `pwd`
    if rvm --version 2>/dev/null; then
      gem install soloist
    else
      sudo gem install soloist
    fi

    mkdir -p ~/cookbooks; cd ~/cookbooks
    cat > soloistrc <<EOF
    cookbook_paths:
    - $PWD
    recipes:
    - pivotal_workstation::ack
    - pivotal_workstation::activemq
    - pivotal_workstation::add_ops_user
    - pivotal_workstation::ccmenu
    - pivotal_workstation::chrome
    - pivotal_workstation::create_var_chef_cache
    - pivotal_workstation::defaults_fast_key_repeat_rate
    - pivotal_workstation::default_editor
    - pivotal_workstation::dropbox
    - pivotal_workstation::enable_assistive_devices
    - pivotal_workstation::erlang
    - pivotal_workstation::firefox
    - pivotal_workstation::flycut
    - pivotal_workstation::function_keys
    - pivotal_workstation::git
    - pivotal_workstation::github_for_mac
    - pivotal_workstation::gitx
    - pivotal_workstation::git_config_global_defaults
    - pivotal_workstation::git_scripts
    - pivotal_workstation::ghostscript
    - pivotal_workstation::global_environment_variables
    - pivotal_workstation::homebrew
    - pivotal_workstation::heroku_toolbelt
    - pivotal_workstation::imagemagick
    - pivotal_workstation::increase_shared_memory
    - pivotal_workstation::inputrc
    - pivotal_workstation::iterm2
    - pivotal_workstation::java
    - pivotal_workstation::keyboard_preferences
    - pivotal_workstation::keycastr
    - pivotal_workstation::librsvg
    - pivotal_workstation::locate_on
    - pivotal_workstation::memcached
    - pivotal_workstation::menubar_preferences
    - pivotal_workstation::menumeters
    - pivotal_workstation::mongodb
    - pivotal_workstation::mouse_locator
    - pivotal_workstation::mysql
    - pivotal_workstation::nginx
    - pivotal_workstation::node_js
    - pivotal_workstation::pango
    - pivotal_workstation::postgres
    - pivotal_workstation::prince_xml
    - pivotal_workstation::qt
    - pivotal_workstation::rabbitmq
    - pivotal_workstation::redis
    - pivotal_workstation::rubymine
    - pivotal_workstation::rubymine_preferences_pivotal
    - pivotal_workstation::rvm
    - pivotal_workstation::screen_sharing_app
    - pivotal_workstation::sizeup
    - pivotal_workstation::skype
    - pivotal_workstation::sshd_on
    - pivotal_workstation::ssl_certificate
    - pivotal_workstation::sublime_text
    - pivotal_workstation::textmate
    - pivotal_workstation::textmate_bundles
    - pivotal_workstation::textmate_preferences
    - pivotal_workstation::tmux
    - pivotal_workstation::unix_essentials
    - pivotal_workstation::user_owns_usr_local
    - pivotal_workstation::vagrant
    - pivotal_workstation::vim
    - pivotal_workstation::virtualbox
    - pivotal_workstation::workspace_directory
    - pivotal_workstation::xquartz
    - pivotal_workstation::z
    EOF
    if [[ -d pivotal_workstation ]]; then
      cd pivotal_workstation && git pull && cd ..
    else
      git clone https://github.com/pivotal/pivotal_workstation.git
    fi
    if [[ -d dmg ]]; then
      cd dmg && git pull && cd ..
    else
      git clone https://github.com/opscode-cookbooks/dmg.git
    fi
    soloist
    popd



I'm thinking of checking out this new tool from github called boxen, which uses puppet, and has similar end goal as soloist.  Though I imagine both of them will have the occasionally problem keeping up with bleeding edge of mac os releases.