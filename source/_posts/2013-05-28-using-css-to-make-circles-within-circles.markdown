---
layout: post
title: "Using CSS to make circles within circles"
date: 2013-05-28 09:26
comments: true
categories:
---

The challenge Jeremiah receives from the prospective employer is to make the following image in html/css.

{% img center /images/circles_challenge.png 350 350 'image' 'images' %}

He is allowed to ask questions of course.  How to begin?

    <div class="bigcircle">

      <div class="circle circle1"> 1 </div>

      <div class="circle circle2"> 2 </div>

      <div class="circle circle3"> 3 </div>

      <div class="circle circle4"> 4 </div>

    </div>

After creating the html above we are going to need to make some styles.  The outer div is for our big circle and we have four other divs within for each of our colored circles.

What sort of css would we need to get the outer div to be a big circle?

The final result is here.

http://cdpn.io/xJmsc