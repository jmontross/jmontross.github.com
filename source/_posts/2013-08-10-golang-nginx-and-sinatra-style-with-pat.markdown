---
layout: post
title: "golang nginx and sinatra style with pat"
date: 2013-08-10 15:18
comments: true
categories: golang, nginx
---

Go looks to be a promising language for writing fast code.  Why?  It was designed from ground up to handle the big problems like concurrency, race conditions, and fast compilation.  It's almost like an interpretted language but it's much closer to the metal.  Enough rambling.

Will set up a really simple web server with golang on ubuntu server running nginx.  Assumes you've got nginx setup which is covered in a previous post for running sinatra and nginx.

first ssh into server

    apt-get install golang-go

make sure you click yes and get the language installed

    cd /opt/awesome/go
    go get github.com/bmizerany/pat


There is some example code on https://github.com/bmizerany/pat that I've copied below

    package main

    import (
        "io"
        "net/http"
        "github.com/bmizerany/pat"
        "log"
    )

    // hello world, the web server
    func HelloServer(w http.ResponseWriter, req *http.Request) {
        io.WriteString(w, "hello, "+req.URL.Query().Get(":name")+"!\n")
    }

    func main() {
        m := pat.New()
        m.Get("/hello/:name", http.HandlerFunc(HelloServer))

        // Register this pat with the default serve mux so that other packages
        // may also be exported. (i.e. /debug/pprof/*)
        http.Handle("/", m)
        err := http.ListenAndServe(":12345", nil)
        if err != nil {
            log.Fatal("ListenAndServe: ", err)
        }
    }


Basically the above code is running a webserver on port 12345 of the localhost and will respond to http get requests at /hello

To run this code we can touch a file called go.go and copy the above into said file and save it.
Now run the file in the background.

     go run go.go &

Test that it works.

     curl -X GET localhost:12345/hello/foo
     hello, foo!

Cool, it works.    Maybe see if it responds to posts.

     root@joshuamontross:/opt/awesome/go# curl -X POST localhost:12345/hello/foo
     Method Not Allowed

That's nice.  Error handling works.  Now let's make nginx serve this page by modifying the awesome config file.

All needs to be added to the location block is the following proxy pass to localhost on the port specified in our program.

    open up the nginx file in sites available.

    sudo vi /etc/nginx/sites-available/awesome

    add in the new /hello route.

    server {
        #listen   80; ## listen for ipv4; this line is default and implied
        #listen   [::]:80 default ipv6only=on; ## listen for ipv6

        # Make site accessible from http://localhost/
        server_name localhost;
        ## make it so we can do curl -X GET localhost/hello/foo

        location /ga/circles {
          alias /usr/share/nginx/www/ga/circles;
        }

        location /hello {
          proxy_pass http://localhost:12345/hello;
        }

        location / {
          proxy_pass http://localhost:4567/;
        }
    }

Save and exit... ":wq" in vim.  And restart nginx

    sudo service nginx restart

Now the site should work from straight localhost without a port being specified since nginx will serve on localhost and proxy pass to our site....

    curl -X GET localhost/hello/foobar
    hello, foobar!

That worked nicely.  visiting joshuamontross.com/hello/whatever


