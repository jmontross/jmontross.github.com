---
layout: post
title: "The difference between n^2 and 2n algorithm example"
date: 2013-01-26 07:34
comments: true
categories:
---
Good morning... how about we do an algorithm together... it's right here in my google docs...  And I'm linked to this question

    Given an integer x and an unsorted array of integers,
    describe an algorithm to  determine  whether two of the numbers add up to x.

Type or speak? I write to him as we are on phone and viewing same screen... he says typing is fine... so i type out this pseudo code of sorts.

iterate over the unsorted array, and for each number iterate over the rest adding them and checking if it is equal to our given integer.  if it is equal to given integer return the two numbers

he says.. that's got an efficiency of n^2 as shown below...

(n-1) * n  => n^2

That's not very efficient says the interviewer from a silicon valley startup that will go unnamed.

In actual ruby code, which I didn't write as I attempted to write the faster solution than n^2... though I was clueless
    def awesome(unsorted_array,integer_x)
      unsorted_array.each_with_index do |item, first_index|
         unsorted_array.each_with_index do |other_item, second_index|
           if (item + other_item == integer_x) && (first_index != second_index)
           return [item, other_item]
           end
         end
      end
     end

awesome [1, 2, 5, 6], 7
 => [1, 6]

How do we make this faster than n^2?

The hint is to use hashes?  Why use a hash?

    def more_awesome(unsorted_array,integer_x)
      desired_addition = {}
      unsorted_array.each_with_index do |item, first_index|
        desired_addition[integer_x - item] = first_index
      end

      unsorted_array.each_with_index do |other_item, second_index|
        if (desired_addition[other_item]) && (desired_addition[other_item] != second_index)
          return [integer_x - other_item, other_item]
        end
      end

     end

This solution is 2N because we loop over the array twice and no more to find if there is an answer.

Let's see if it's actually faster....

    require 'benchmark'

    unsorted_array = [1,2,5,6]
    integer_x = 7
    Benchmark.bm do |x|
      x.report { more_awesome unsorted_array,integer_x }
      x.report { awesome unsorted_array,integer_x }
    end
       user     system      total        real
       0.000000   0.000000   0.000000 (  0.000036)
       0.000000   0.000000   0.000000 (  0.000018)

Why isn't it faster?

I think it might be because the sample is too small.  Our sample data set used small list and in this case the less efficient solution executes faster  due to the fact that the answer is a combination of the first and last item, so the result is found after only 3 computations because we first loop over the 1, and then add the 2 to see if it is 7, then we check if 1 + 5 is seven, then we return 1 and 6 because they equal 7.

In the "more efficient" solution we are only more efficient in the worst case scenario, which in the case of the above answers would be if we wanted 11.

    unsorted_array = [1,2,5,6]
    integer_x = 11

     Benchmark.bm do |x|
       x.report { more_awesome unsorted_array,integer_x }
       x.report { awesome unsorted_array,integer_x }
     end

         user     system      total        real
     0.000000   0.000000   0.000000 (  0.000024)
     0.000000   0.000000   0.000000 (  0.000020)

Looks really close.... but still not faster?  Maybe it's because the array is just too small and the time it takes to build the hash just doesn't cut it.

    unsorted_array = [1,1,1,1,1,1,2,5,6]
    integer_x = 11
    ## same benchmark
        user     system      total        real
     0.000000   0.000000   0.000000 (  0.000040)
     0.000000   0.000000   0.000000 (  0.000054)

Nice. We got our more efficient solution to actually run faster.  Now let's stop making contrived tests to make our answer seem right and use something a little more fair.

    require 'benchmark'

    unsorted_array = (1..10000).map { rand(10000) }
    integer_x = rand(10000)

    Benchmark.bm do |x|
       x.report { more_awesome unsorted_array,integer_x }
       x.report { awesome unsorted_array,integer_x }
    end

      user     system      total        real
    0.000000   0.000000   0.000000 (  0.007244)
    0.010000   0.000000   0.010000 (  0.011289)

