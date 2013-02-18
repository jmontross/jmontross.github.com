---
layout: post
title: "given unsorted array and integer x part 3"
date: 2013-02-14 02:10
comments: true
categories:
---
 After sending over my results from <a href="http://jmontross.github.com/blog/2013/01/26/given-unsorted-array-and-integer-x-part-2/"> part 2 </a> of the unsorted array and integer x problem, I got no response from this mysterious interviewer... and I imagined it's because my solution without hashes was terribly inefficient.  It must be possible to solve this problem without hashes elegantly and more efficiently.

 I've decided to go with a new solution where we sort the array from the start then build a list of the target answers, similarly to the building up of the hash, except we do this in constant time (I think) and keep our list tidy with only the minimal possible answers remaining in our sorted array of integers.

    def adds_to_x?(array,integer_x)
      answers = []
      array.sort
      array.each do |item|
        if answers.last == item
          return [integer_x - item, item]
        end
        answers << (integer_x - item)

        while answers.first && answers.first < item
          answers.shift
        end
        if answers.first == item
          return [integer_x - item, item]
        end
      end
    end

    Benchmark.bmbm do |x|
       x.report { more_awesome unsorted_array,integer_x }
       x.report { awesome unsorted_array,integer_x }
       x.report { adds_to_x? unsorted_array,integer_x }

    end
           user     system      total        real
           0.590000   0.000000   0.590000 (  0.601332)
           0.370000   0.010000   0.380000 (  0.367863)
           0.300000   0.000000   0.300000 (  0.297452)

We will look into just how efficient each of these algorithms are soon enough.