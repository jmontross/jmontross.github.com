---
layout: post
title: "given unsorted array and integer x part 2"
date: 2013-01-26 12:27
comments: true
categories: algorithms, ruby
---

    Given an integer x and an unsorted array of integers,
    describe an algorithm to  determine  whether two of the numbers add up to x.

I tell this mysterious hiring manager about my new implementation to the above solution <a href="{{ page.previous['url'] }}"> here </a> that uses hashes and is faster than n^2 and he says,

    "Hashed are good shortcuts. But can you do it without hashes?"

I think to myself.... unsorted array and I'm looking for sums

I could first sort the array.  Then I could do a binary search?
I could even optimize it for only positive integers....

I'm a little unsure.  Anyone have any ideas?

     def even_more_awesome?(unsorted_array,integer_x)
       sorted_array = unsorted_array.sort
       # at this point I could search the array
         # to see if there is
         # an upper bound where my integer_x exists

         sorted_array.each_with_index do |item, index|
             target = integer_x - item
           ## not sure what to do here?
             if item > integer_x
               return false
             end
           if sorted_array[index+1,sorted_array.length-1].include?(target)
               return [item, target]
             end
         end

        end

         even_more_awesome? [1,3,5], 8
     => [3, 5]

it works but it could be better...

    def even_more_awesome?(unsorted_array,integer_x)
      sorted_array = unsorted_array.sort
      sorted_array.each_with_index do |item, index|
        target = integer_x - item
        if item > integer_x
          return false
        end

        for i in sorted_array[index+1,sorted_array.length-1]
          break if i > target
          return [item, target] if i == target
        end

       end
       return false
     end

     even_more_awesome? [1,3,5], 8
     => [3, 5]


Now it works... I wonder how it compares to my other solutions?  I also wonder if my earlier test was very biased.  I decided to use a larger random data set and to run it twice with bmbm instead of bm.


    require 'benchmark'

    Benchmark.bmbm do |x|
       unsorted_array = (1..10000).map { rand(10000) }
       integer_x = rand(10000)
       x.report { more_awesome unsorted_array,integer_x }
       x.report { awesome unsorted_array,integer_x }
       x.report { even_more_awesome? unsorted_array, integer_x}
    end


     user     system      total        real
    0.000000   0.000000   0.000000 (  0.003523)
    0.000000   0.000000   0.000000 (  0.000252)
    0.010000   0.000000   0.010000 (  0.002643)

     user     system      total        real
    0.000000   0.000000   0.000000 (  0.004666)
    0.000000   0.000000   0.000000 (  0.002127)
    0.000000   0.000000   0.000000 (  0.001775)


Now I'm just getting crazy but I want it to be better...

    def faster?(unsorted_array,integer_x)
      sorted_array = unsorted_array.sort
      sorted_array.each_with_index do |item, index|
        target = integer_x - item
        if item > integer_x
          return false
        end

        max = sorted_array.length-1
        next_index = index+1
        for i in sorted_array[index+1,max]
          max = next_index && break if i > target
          return [item, target] if i == target
          next_index +=1
        end

       end
       return false
     end

I figured that this should be fastest, and my results from before may be biased because the arrays were too small.  I made them 100 times larger and moved the array declaration and integer_x outside the bmbm block.


    require 'benchmark'
    unsorted_array = (1..1000000).map { rand(1000000) }
    integer_x = rand(1000000)

    Benchmark.bmbm do |x|
       x.report { more_awesome unsorted_array,integer_x }
       x.report { awesome unsorted_array,integer_x }
       x.report { even_more_awesome? unsorted_array, integer_x}
       x.report { faster?  unsorted_array, integer_x }
    end


    user     system      total        real
    0.620000   0.010000   0.630000 (  0.632894)
    0.270000   0.000000   0.270000 (  0.266455)
    0.340000   0.000000   0.340000 (  0.340122)
    0.340000   0.000000   0.340000 (  0.349022)


    user     system      total        real
    0.640000   0.010000   0.650000 (  0.644997)
    0.270000   0.000000   0.270000 (  0.269851)
    0.340000   0.000000   0.340000 (  0.348532)
    0.350000   0.000000   0.350000 (  0.354552)

It looks like I went down the wrong worm hole... All my newest results are much slower and are simply two variations of more inefficient solutions.  I'll need to come back to this...