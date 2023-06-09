---
title: The $100,000 Keying Error
date: 2008-06-11T16:39:56+00:00
pvc_views:
  - 15247
dsq_thread_id:
  - 179878237
categories:
  - Development

---
The [IEEE summary of this error][1] is worth a read, both from the cause and the outcome of the UI testing.

Since I&#8217;m frequently dealing with interfaces that require typing of sets of digits or numbers.&nbsp; I&#8217;ve been thinking about this for the last few days and have two thoughts on how to reduce errors like this.&nbsp; Not of course that I have any capabilities to test either idea, but there&#8217;s always a chance that somebody has some additional insight.

The core summary of the error is:

> Fossbakk&#8217;s daughter&#8217;s account number was 71581555022, but she inserted an extra 5 and keyed in 71581555 5022. The user interface accepted only 11 digits in this field (the standard length of a Norwegian account number), thus truncating the number to 71581555502. The last digit is a checksum based on a modulo-11 formula. This will detect all single keying errors and errors where two consecutive digits are interchanged. Inserting an extra 5 changed both the ninth and tenth digits.

Ideas of how to reduce the error: 

**Grouping of digits**

Telephone numbers most specifically, are traditionally written in different grouping combinations (nn nn nn nn OR nnn nnn nnnn).&nbsp; While bank numbers are&nbsp; usually just strings of digits.&nbsp; Thus providing very few visual cues that there is an error, in a grouped system you would notice that the first digit of a group is incorrect.&nbsp;&nbsp; _Though I will say that windows product keys, which are grouped are a total pain, but they at least provide a quick way to see that any one group is ok._

**Mixed digits and alpha** 

If a account number was a mixture of digits an alphabetic characters (avoiding those that might be misleading 0 vs. O), I would imagine that similar to the grouping strategy it would reduce the error rate.&nbsp; Things like:&nbsp;&nbsp; 715 BUVX 5022, one could even extend this to be similar to the old US telephone system nomenclature &#8220;715 wabash 5022&#8221; where technically only the &#8220;WAB&#8221; is the unique part, but by turning into a real word the error rate might be reduced.

 [1]: http://www.computer.org/portal/site/computer/menuitem.5d61c1d591162e4b0ef1bd108bcd45f3/index.jsp?&pName=computer_level1_article&TheCat=1015&path=computer/homepage/0408&file=profession.xml&xsl=article.xsl
