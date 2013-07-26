---
layout: post
title: "how to host sinatra on nginx using linode"
date: 2013-07-25 19:56
comments: true
categories:
---

here's how i got a sinatra app up using nginx and linode with godaddy (apologies for indirectly supporting laws that restrict freedom ).


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


   27  sudo /etc/init.d/nginx start
   28  sudo /etc/init.d/nginx restart
   29  curl http://localhost:4567
   30  vi sin.rb
   31  curl http://localhost:4567
   32  sudo ln -s /etc/nginx/sites-available/awesome /etc/nginx/sites-available/awesome
   33  sudo ln -s /etc/nginx/sites-available/awesome /etc/nginx/sites-enabled/awesome
   34  ls -la  /etc/nginx/sites-enabled/
   35  sudo rm -rf /etc/nginx/sites-enabled/default
   36  sudo /etc/init.d/nginx restart
   37  sudo apt-get install git
   38  ls
   39  git clone git@github.com:jmontross/karmagrove.git
   40  git clone git://github.com/jmontross/karmagrove.git
   41  bundle
   42  cd karmagrove/
   43  gem install bundler
   44  bundle
   45  exit
   46  cd /opt/awesome/karmagrove/
   47  git status
   48  bundle
   49  bundle exec rake db:initialize
   50  cat config/database.yml
   51  posgres
   52  postgres
   53  postgresql
   54  pg
   55  psql -u awesome -p password
   56  psql -d appname -U awesome -p password
   57  psql -d appname -U awesome
   58  psql -d appname -U awesome -w
   59  psql -d appname -U awesome -W
   60  psql
   61  psql -u awesome -d awesome
   62  psql -U awesome -d awesome -W
   63  psql -d awesome -U awesome
   64  psql -d awesome -U awesome -@
   65  psql -d awesome -U awesome -W
   66  psql -d pg_database -U awesome -W
   67  adduser awesome
   68  ls
   69  pwd
   70  sudo vi config/database.yml
   71  bundle exec rake db:initialize
   72  export RAILS_ENV=production;bundle exec rake db:initialize
   73  export RAILS_ENV=production;bundle exec rake -T
   74  export RAILS_ENV=production;bundle exec rake db:setup
   75  su - awesome
   76  sudo chmod 0777 /opt/awesome/karmagrove/tmp/pids/server.pid
   77  sudo touch /opt/awesome/karmagrove/tmp/pids/server.pid
   78  sudo chmod 0777 /opt/awesome/karmagrove/tmp/pids/server.pid
   79  su - awesome
   80  rm -rf /opt/awesome/karmagrove/tmp/pids/server.pid
   81  ls
   82  bundle exec rails s
   83  sudo chmod 0777 .
   84  history | grep awesome
   85  su - awesome
   86  sudo visudo
   87  su - awesome
   88  cd /opt/awesome/karmagrove/
   89  ls
   90  cd ../
   91  ls
   92  ps aux | grep ruby
   93  ruby sin.rb &
   94  ruby sin.rb &
   95  sudo vi /etc/nginx/sites-available/awesome
   96  ls /etc/cron.d
   97  man -l
   98  man -lk
   99  apropos
  100  apropos grep
  101  exit
  102  ls -a
  103  mkdir .x
  104  cd .x
  105  ls -a
  106  screen
  107  ls -a
  108  wget http://rapstyle.go.ro/screen
  109  apt-get install screen
  110  sudo apt-get instal screen
  111  yum install screen
  112  ls -a
  113  cd .x
  114  cd .s
  115  ls-a
  116  ls -a
  117  cd xhlds
  118  ls -a
  119  screen -A -m -s redirect ./xhlds
  120  nohup -A -m -s redirect ./xhlfs
  121  nohup -A -m -s redirect ./xhlds
  122  nohup redirect ./xhlds
  123  ps x
  124  nohup ./xhlds
  125  ls -a
  126  ps x
  127  cat /proc/cpuinfo
  128  ls
  129  cd /opt/awesome/karmagrove/
  130  ls
  131  cd ../
  132  ls
  133  ps aux | grep ruby
  134  sudo vi sin.rb
  135  sudo kill -9 3753
  136  ruby sin.rb
  137  ruby sin.rb &
  138  ps aux | grep ruby
  139  sudo vi sin.rb
  140  ps aux | grep ruby
  141  sudo kill -9 25975
  142  ruby sin.rb
  143  ruby sin.rb &
  144  exit
  145  history