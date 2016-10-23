---
layout: post
title: "Writing a stubbing function in R"
date: 2016-10-23
---

Stubbing is always an interesting business. This is the practice of exchanging a 'real' function for another one, usually for testing purposes. When a function is stubbed out, the code that calls it doesn't actually change, but it _somehow_ is compelled to call the alternative function.

 In compiled languages, it's very difficult and very messy to stub function out, which has led to the use of the inversion of control principles to ensure testability. Instead of stubbing out a function, we just make sure to pass anything we may want to stub out as a parameter.

In interpreted languages, though, it's often possible to stub out functions at runtime. A great example of this is python's [mock library](https://docs.python.org/3/library/unittest.mock.html).

In this post I'll take a look at how to write a stubbing function in R. This functionality is available as part of the [mockery](https://github.com/n-s-f/mockery) package, which will soon be available on CRAN.

### In the beginning

### Namespaced functions

There are a few good resources online about how R namespacing and scoping work. The most helpful to me was [this one](http://blog.obeautifulcode.com/R/How-R-Searches-And-Finds-Stuff/) by Suraj Gupta. If you stare at all the lines and charts and graphs for long enough, you'll eventually see that there isn't any way to intercept a call to a namespaced function. Calling a namespaced function tells R exactly where to look, so changing something in the scope of the test function as we did above just isn't going to work. We _could_ reload the namespace with our stub function in place of the original, but then we'd have to reload the original namespace. The test runner shouldn't have to do that for us, and leaving that up to the developer is bound to lead to problems.

So it seems like we might be stuck.

Fortunately, there's a way out.

It turns out that the `::` operator that R uses to refer to namespaced functions is actually a function itself. And this means, of course, that we can stub it out using the code we've developed above.

How does that help us? Well, it means that even though we can't intercept the call to a namespaced function after it's made, we _can_ stop it from ever getting made in the first place.

One thing we could do is stub out the `::` to simply mangle the name of a namespaced function call.
