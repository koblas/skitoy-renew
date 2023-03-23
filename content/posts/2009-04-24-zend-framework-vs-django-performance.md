---
title: Zend Framework vs. Django Performance
date: 2009-04-24T16:04:55+00:00
categories:
  - Development
tags:
  - django
  - php
  - zend
---

This is not a scientific nor rigorious test&#8230;  But, here&#8217;s some interesting data for people to chew on.  I&#8217;ve got a production site built using the Zend Framework and a beta site built using django.  What&#8217;s interesting is the &#8220;Time spent downloading a page&#8221; graphs from Google Webmaster Tools.

The Zend Framwork Graph &#8212; average speed is 458 ms with a min of 246 ms

<img title="wink_time" src="/posts/wink_time.png" alt="wink_time" width="500" height="150"/>

The django graph &#8212; you can see where I changed from CGI style to FCGI in the speed &#8212; average is 531, and a min is 81ms &#8212; though my eyeball average for the month of April is 100ms.

<img src="/posts/zapquiz_time1.png" alt="zapquiz_time1" width="500" height="150" />

The nice part is these graphs have the same scale, so it&#8217;s pretty clear that once I switched from CGI to FCGI (under lighttpd) that the django performance is 4x better than the ZF.
