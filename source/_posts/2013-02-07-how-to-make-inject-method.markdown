---
layout: post
title: "how to make inject method"
date: 2013-02-07 18:31
comments: true
categories:
---
This interviewer is like, what is your ruby skill on 1 to 10 and I'm like... I guess if Matz is 10 and newbie who just heard about ruby is 1, I'm like a 7.

He goes, okay, so what if we didn't have the inject method, can you make it?

I'm thnking, this shouldn't be so hard... I know how inject works.
It is an enumerable method that can have an initializer and takes a block, and returns a single value.  For instance


    [1,2].inject(1) { |initializer,item| initializer + item }
    ## first it comes across the item of 1 and ads 1 to it, then the result
    ## is 2, which is the initializer that is then added to 2, giving us
    => 4

So now that we know how inject works, let's implement it!


    def inject(initializer, &block)
       # I think I'll need to store the initializer
       response = initializer
       self.each do |item|
         ## How do I yield to the block to build the response
         ## response = use_my_block(response,item)
       end
       response
    end

I mention to the interviewer that I'm thining I am probably more like a 6 now...He chuckles and says many a time he's taken a 7 down to a 4.  He asks me if I've heard of the yield method, which fixes my method_missing above


    def inject(initializer, &block)
       response = initializer
       self.each do |item|
         response = yield response, item
       end
       response
    end


And imagine that is inside the Array class...

To make it a little nicer and refactor a bit.

    class Array
      def inject(initializer, &block)
       self.each do |item|
         initializer = yield initializer, item
       end
       initializer
      end
    end

Let's test it out and see if our new inject works the same as the real implementation that was shown at beginning...

    [1,2].inject(1) { |initializer,item| initializer + item }
    => 4


