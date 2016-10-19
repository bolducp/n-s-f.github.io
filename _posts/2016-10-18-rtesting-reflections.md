---
layout: post
title:  "Automated testing in statistical analysis"
date:   2016-10-18
---

I recently gave a very introductory talk to a group of biostatistics students about automated testing in R. The talk did little more than explain what automated testing is, why you might want to do it, and how to go about it in R ([slides](https://n-s-f.github.io/talks/rtesting.html) and [starter testing repository](https://github.com/n-s-f/testthat-starter) are available online).

This got me thinking about the state of automated testing in statistical software. I'm fortunate to be in a department that emphasizes statistical computing for its own students, and is also spreading statistical computing knowledge through its massively popular online courses. And yet, as far as I'm aware, the idea of automated testing is barely if at all covered in any of the computing courses.

Part of the reason for this has to do with the nature of statistical analysis. There isn't really a way to formally test if an analysis is _right_, so it may seem like automated testing in statistical software is moot. It's also true that some of the greatest benefits of testing are proportional to the size of the code base and the number of person-hours spent on the project. While the number of software developers touching the same code in a short period can be very large, the same doesn't tend to hold for statisticians. Statistical analyses also tend to reach a point where they are _done_. Nobody is continuously using them and they do not have to be maintained.

That said, the large majority even of statistical code is not direclty involved in the analysis. Instead it's concerned with all the many and varied parts of data munging. There's also a great deal of code for statistical methedologies, which also can and should be tested. There have been a number of cases recently in which failures in both of these elements of statistical software have caused serious problems.

My guess is that because statisticians are often involved in work where the benefits of automated testing are not as significant, they tend not to value the practice as highly as software developers. One side-effect of this is that when statisticians _do_ write code that would dramatically benefit from automated tests, they do not know how to include them effectively.

By contrast, software developers tend to work almost exclusively in conditions that offer very high returns to effort invested in automated tests. They've invested a lot of effort in thinking about how to right tests effectively and in building great testing tools that integrate nicely into their development environments. While testthat is a really tremendous step forward for testing in R, it also makes a few design choices that make it a little difficult for beginners to get started with, especially those who primarily use RStudio.

Statisticians in part have a hard time taking advantage of the progress made in automated testing on the software side because they tend to use R. I think perhaps more importantly though it's because the conceptual framework and tools built up around testing are optimized for software use cases rather than statistical use cases. I'm curious what would happen if we were to rebuild the concepts and tools around automated testing to optimize for computational statistics.
