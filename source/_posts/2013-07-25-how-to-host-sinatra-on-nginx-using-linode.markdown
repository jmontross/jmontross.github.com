---
layout: post
title: "how to host sinatra on nginx using linode"
date: 2013-07-25 19:56
comments: true
categories: nginx, ruby, linode, godady
---

here's how i got a sinatra app up using nginx and linode with godaddy (apologies for indirectly supporting laws that restrict freedom ).  What follows is my command line history from the moment I got my linode until I had sinatra serving some files with the help of nginx.


    1  ls
    2  pwd
    3  ruby -v
    4  sudo apt-get install ruby1.9.1
    5  sudo apt-get install nginx
    6  sudo mkdir /opt/awesome
    7  cd /opt/awesome/
    8  touch sin.rb
    9  gem install sinatra
    10  add-apt-repository ppa:nginx/stable
    11  apt-get install python-software-properties
    12  echo "joshuamontross.com" > /etc/hostname
    13  hostname -F /etc/hostname

Now update apt-get and install nginx

    14  apt-get update
    15  apt-get upgrade --show-upgraded
    16  apt-get install nginx
    17  sudo /etc/init.d/nginx start
    18  curl http://localhost:80
    19  ls
    20  vi sin.rb
    21  ruby sin.rb
    22  ruby sin.rb &
    23  ls
    24  vi sin.rb
    25  sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/awesome
    26  sudo vi /etc/nginx/sites-available/awesome

this is what my awesome looks like....

     server {
        #listen   80; ## listen for ipv4; this line is default and implied
        #listen   [::]:80 default ipv6only=on; ## listen for ipv6

        root /usr/share/nginx/www;
        index index.html index.htm;

        # Make site accessible from http://localhost/
        server_name localhost;

              location /ga/circles {
                   alias /usr/share/nginx/www/ga/circles;
              }

              location / {
          # First attempt to serve request as file, then
          # as directory, then fall back to index.html
                      proxy_pass http://localhost:4567/;
                      #try_files $uri $uri/ /index.html;
          # Uncomment to enable naxsi on this location
          # include /etc/nginx/naxsi.rules
        }
      }

Now restart nginx.

    28  sudo /etc/init.d/nginx restart

Site should be up at our port directly hitting sinatra and via hitting nginx at localhost

    29  curl http://localhost:4567
    => oooooh yeah <br /> ... maybe you want my "portfolio" site at <a href="http://www.pointmanj.com"> pointmanj.com </a>

    curl http://localhost
    => oooooh yeah <br /> ... maybe you want my "portfolio" site at <a href="http://www.pointmanj.com"> pointmanj.com </a>

symlink the available site to sites-enabled.
    33  sudo ln -s /etc/nginx/sites-available/awesome /etc/nginx/sites-enabled/awesome
    34 ls -la  /etc/nginx/sites-enabled/

    lrwxrwxrwx 1 root root   34 May  9 03:49 awesome -> /etc/nginx/sites-available/awesome

Remove default nginx file.

    35  sudo rm -rf /etc/nginx/sites-enabled/default
    36  sudo /etc/init.d/nginx restart

Make sure to log into godaddy or whatever and point the @record to the ip of your linode webserver.

{% img center /images/godaddy_linode.png 1000 1000 'image' 'images' %}

This may take a little bit to propogate through the interwebs.... but eventually

Now the site is being served at /, which is joshuamontross.com

I'd like to get a rails site up, but first I'll need git.
    37  sudo apt-get install git

Now to get my rails app.

    39  git clone git@github.com:jmontross/karmagrove.git

This will have to be another post... Hope setting up nginx, sinatra, and