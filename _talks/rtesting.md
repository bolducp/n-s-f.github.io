---
layout: talk
title: "Automated testing in R"
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


<section>

<h4>What is Automated Testing</h4>

<section data-markdown>

- Code that runs and checks itself

- Run functions with a variety of inputs and check outputs

- Replaces reloading source and checking outputs manually
</section>

<section data-markdown>

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
<h2>Why test?</h2>

<ol>
<span class="fragment">
<li>Improve reliability</li>
</span>
<span class="fragment">
<li>Facilitates refactoring</li>
</span>
<span class="fragment">
<li>Improves code design</li>
</span>
<span class="fragment">
<li>Makes large projects manageable</li>
</span>
</ol>

</section>

</section>

<section>

<h4>Testing Framework</h4>

<section>

<ul>
<li>testthat is the best testing framework in R</li>
<li>tightly coupled to devtools</li>
<li>fits nicely with the rest of the tidyverse</li>
<li>--(if you are into that kind of thing)</li>
</ul>

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

#### testthat-starter

Starter kit for playing with automated tests: 

```
git clone https://github.com/n-s-f/testthat-starter.git
```
</section>
<section>
<center>{ { coding break } }</center>
</section>
</section>

<section>
<h4>Testing Best Practices</h4>
<section data-markdown>

How to write a test

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
- Write tests and code at the same time
</span></p>

<p><span class="fragment">
- Tight feedback loop
</span></p>

<p><span class="fragment">
- Encourages clear thinking and good design
</span></p>

</section>

<section>
<br>
<h2>Mocking</h2>
Two use cases:
<ol>
<li>Isolate code for testing</li>
<li>Remove calls to slow functions or external resources</li>
</ol>
</section>
<section>
<br>
<h2>Mocking</h2>
<span class="fragment">
<p>Remember: unit tests test one unit</p>
</span>
<span class="fragment">
Mocking helps isolate code so a test is not running too much code.
</span>
</section>
<section>
<br>
<h2>Mocking</h2>

<pre><code data-trim>
test_that('something involving slow function', {
    with_mock(slow_function=function(...) {return(5)}, {
        result = function_that_calls_slow_function()
        expect_equal(result, 123)
    })
})
</code></pre>
 
</section>

<section>
<br>
<h2>Testability</h2>
<span class="fragment">
<p>Functions! (not function-free scripts)</p>
</span>
<span class="fragment">
<p>Small functions</p>
</span>
<span class="fragment">
<p>    (like, really really small)</p>
</span>
</section>

<section>
<br>
<h2>Testability</h2>
<span class="fragment">
<p>Pass data into functions so it can be tested with different inputs</p>
</span>
<span class="fragment">

Good:
<pre><code data-trim>
magic = function(col_name, some_data) {
    ## do some important work
    return(1)
}

get_data = function(data_location) {
    # find the dataz
}

data = get_data('the place')
magic('the column', data)
</code></pre>
</span>
<span class="fragment">
You can pass in arbitrary data for testing purposes (edge cases, etc.)
</span>
</section>

<section>
<span class="fragment">

Less good:
<pre><code data-trim>
magic = function('col_name') {
    data = get_data_from_somewhere_hardcoded()
    ## do some important work
    return(1)
})
</code></pre>
 
</span>
<span class="fragment">
Cannot test `magic` with arbitrary data.
</span>
</section>

<section>
<br>
<h2>Functional Programming</h2>

<span class="fragment">
<p>Everything is a function</p>
</span>

<span class="fragment">
<p>Easier to test inputs and outputs</p>
</span>

</section>
<section>
You can go much deeper...
</section>
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
