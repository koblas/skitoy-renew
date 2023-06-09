---
title: JavaScript version of the Perl/PHP performance test
date: 2008-06-13T19:37:24+00:00
pvc_views:
  - 3164
dsq_thread_id:
  - 171677252
categories:
  - Development
tags:
  - performance

---
Using the Rhino compiler&#8230; sorry no benchmark numbers, but just as a placeholder. To pick a nit, wanted to do ```first[m[1]] += parseInt(m[3])``` but that yeilded a NaN since ```first[m[1]]``` was undefined... You would think that JavaScript would make undefined == 0.

```java
fd = java.io.BufferedReader(java.io.FileReader('p.test'))

mre = /^__MULTI_TOKEN__\s+(\S+)\s+(.*)\t?\s*(\d+)\s*$/;
sre = /^__SINGLE_TOKEN__\s+(\S+)\s*\t?\s*(\d+)\s*$/;

ofd = java.io.PrintWriter(java.io.FileWriter('full.txt'));
first = {}

while (line = fd.readLine()) {
    if (m = line.match(mre)) {
        first[m[1]] = (first[m[1]] ? first[m[1]] : 0) + parseInt(m[3]);
        ofd.println(m[1] + " " + m[2] + "\t" + m[3]);
    } else if (m = line.match(sre)) {
        first[m[1]] = (first[m[1]] ? first[m[1]] : 0) + parseInt(m[2]);
    } else {
        print("Unknown: " + line);
    }
}

ofd.close()

ofd = java.io.PrintWriter(java.io.FileWriter('first.txt'));
//  Sigh... rhino needs an update...
//   first.forEach(function(e, i, a) { ofd.println(i + "\t" + e); });
for (key in first) {
    ofd.println(key + "\t" + first[key]);
}
ofd.close()
```

This is all based on the <a href="http://www.skitoy.com/p/performance-of-python-php-and-perl/160">PHP/Perl/Python</a> performance code.
