---
layout: post
title: "airpair - remote pairing with strangers for fun and profit"
date: 2013-02-22 13:13
comments: true
categories:
---

Recently I had the pleasure of trying out a new pairing service called airpair.co

I sign up for the developer path and list the host of technologies I feel comfortable philosophizing about over a pseudo-anonymous pairing service.

A nice chap named Jonathan contacts me and asks if I could comment on the code at this gist below

https://gist.github.com/anonymous/d9cb557b84a16088c64e

I look at it and end up thinking I could add some value there.  There is a fat controller, and I can only imagine what the model and routes are - and because I'd like to see this model and routes file I suggest I could add some value.

Somehow I find myself doing a google hangout with two strangers discussing a rails controller and how we may go about refactoring it.  We all agree it is a bit of a mess - just too much going on.  How do we improve it?

One thing we begin to notice is that the methods / controller actions are not always mapped to routes and appear to be doing things to the model.  For instance, there is a controller method like so

    def event_list
        @events = current_company.events
        @communication = if params[:communication_id].to_i > 0
          current_company.communications.find(params[:communication_id])
        else
          Communication.new
        end
        render layout: false
    end

The above could easily become a method on the communications model.

Then we thought about the idea of creating a new controller that is subclassed from communications.  This is useful because these actions do seem to be acting on a different type of a communication.  We went through and played around with making the communications a little more restful, and it was a fun experience I'd love to do again.  Next time I think I'd like to use some tmux and actually dig into the code together... similar to how we did earlier <a href="{{ page.previous['url'] }}"> here </a>

Modified controller => https://gist.github.com/anonymous/ce917ac4867b7de57bc9

