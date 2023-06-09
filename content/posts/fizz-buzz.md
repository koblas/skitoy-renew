---
title: Fizz Buzz
date: 2012-10-05T03:13:54+00:00
aktt_notify_twitter:
  - yes
pvc_views:
  - 7819
aktt_tweeted:
  - 1
dsq_thread_id:
  - 871957284
categories:
  - Development
tags:
  - python
  - fizzbuzz

---
As seen on a thread on [Hacker News][1] about Fizz Buzz and &#8220;interesting&#8221; functional ways to solve it.  Realized that there are many ways to boil the ocean, but this feels like a nice compromise between data/program separation and language.

_Note this is using a &#8220;Bazz&#8221; variant of the FizzBuzz problem where Bazz is printed every 7 numbers._

```python
cases = [(3, "Fizz"), (5, "Buzz"), (7, "Bazz")]

for i in range(110):
    pr = ''.join([s * (i % m == 0) for m, s in cases])
    print pr or i
```

 [1]: http://news.ycombinator.com/item?id=4613754
