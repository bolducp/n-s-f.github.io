---
layout: talk
title: "Automated testing in R (in progress...)"
---

<style>
    .reveal .slides {
        text-align: left;
    }
    .reveal .slides section>* {
        margin-left: 0;
        margin-right: 0;
    }
</style>

<section>
<h2>Automated testing in R</h2>

<p><em>Noam Finkelstein</em></p>
<span class="fragment">
Please interupt with questions / comments!
</span>

</section>

<section>
<h2>Outline</h2>
<ol>
 <span class="fragment"><li>Introduction: What is Automated testing?</li></span>
 <span class="fragment"><li>Practical testing in R: How to use testthat</li></span>
 <span class="fragment"><li>Automated testing best practices</li></span>
</ol>
</section>


<section data-markdown>

## What is Automated Testing

- Code that runs and checks itself

- Run functions with a variety of inputs and check outputs

- Replaces constantly reloading source and checking outputs manually
</section>

<section data-markdown>

## What is Automated Testing

Function:

<pre><code data-trim>
    magic = function(a, b) {
        # do some important work
        return(a + b)    
    }
</code></pre>
 
Test:

<pre><code data-trim>
    test_that('test magic adds two numbers', {
        # given 
        num1 = 4
        num2 = 5

        # when
        result = magic(num1, num2)

        # then
        expect_equal(result, 9)
    })
</code></pre>

</section>

<section>

<h2>Testing Framework</h2>

<p>testthat is the best testing framework in R</p>

<p>A few downsides relative to other languages:</p>
<p>- tightly coupled with other 'tidyverse' packages (devtools)</p>
<p>- ponderous setup</p>
<p>- minimal support for mocking and fixtures</p>

</section>

<section data-markdown>

## Testing Framework

Two parts:
- Test Runner (runs test functions)
- Expectations / Assertions

</section>

<section data-markdown>

## Installation

Get it now!

```
>
> install.packages('testthat')
>
```
</section>

<section data-markdown>

## How to write a test

Function:

<pre><code data-trim>
    magic = function(a, b) {
        # do some important work
        return(a + b)    
    }
</code></pre>
 
Test:

<pre><code data-trim>
    test_that('test magic adds two numbers', {
        # given 
        num1 = 4
        num2 = 5

        # when
        result = magic(num1, num2)

        # then
        expect_equal(result, 9)
    })
</code></pre>

</section>

<section>

<h2>Parts of a test</h2>

<p><span class="fragment">
- Setup: # given
</span></p>

<p><span class="fragment">
- Action: # when
</span></p>

<p><span class="fragment">
- Assertion: # then
</span></p>

</section>

<section>

<h2>Levels of Testing</h2>

<p><span class="fragment">
- Unit Testing
</span></p>

<p><span class="fragment">
- Component Testing
</span></p>

<p><span class="fragment">
- Integration Testing
</span></p>

</section>

<section>

<h2>Test Driven Development</h2>

<p><span class="fragment">
- Unit Testing
</span></p>

<p><span class="fragment">
- Component Testing
</span></p>

<p><span class="fragment">
- Integration Testing
</span></p>


</section>

<section>

<h2>open source plug</h2>

<p>Proctor</p>
<p>- test runner</p>
<br>
<p>Mockery</p>
<p>- stubbing and mocking library</p>

</section>

<section>

<h2>Questions?</h2>
</section>
