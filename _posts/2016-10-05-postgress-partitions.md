---
layout: post
title:  Aggregating functions over postgres partitions
date:   2016-10-05
---

Postgres has a number of useful [window functions](https://www.postgresql.org/docs/9.3/static/functions-window.html).

Window functions operate on data that has been partitioned into 'windows'. They apply functions over the records in each window. In this sense, they're a lot like using aggregating functions with `group by`.

The difference is that window functions do not collapse the grouped records into a single record. This means that each record can be aware of where it stands in relation to other records in the window, as we'll see below.

All aggregating functions that can be used with `group by` also work in the context of windows, but no window functions can be used with group by.

I recently encountered some confusing behavior while using the `last_value` aggregating function on windows, so here's a summary of the issue for anyone who runs into the same problem.

Let's look at a quick example. We have the following relation:

| name     |      age      |  grade|
|----------|---------------|-------|
| jack |  10 | 5 |
| john |    10   |  6 |
| nicole | 11 |    5 |
| susan | 11 |    6 |
| tracey | 11 |    7 |

<br>


We can use a conventional aggregating function with a window, as follows:

{% highlight sql %}
  select name, age, grade, avg(grade) over (partition by age)
  from students;
{% endhighlight %}

| name     |      age      |  grade| avg|
|----------|---------------|-------|-------|
| jack |  10 | 5 | 5.5 |
| john |    10   |  6 | 5.5 |
| nicole | 11 |    5 | 6 |
| susan | 11 |    6 | 6 |
| tracey | 11 |    7 | 6 |

<br>

We can also, as mentioned, apply an window function that does not return only one value per window:

{% highlight sql %}
  select name, age, grade,
         row_number(grade) over (partition by age order by grade)
  from students;
{% endhighlight %}

| name     |      age      |  grade| row_number |
|----------|---------------|-------|-------|
| jack |  10 | 5 | 1 |
| john |    10   |  6 | 2 |
| nicole | 11 |    5 | 1 |
| susan | 11 |    6 | 2 |
| tracey | 11 |    7 | 3 |

<br>

Here's where things get interesting. If we provide an explicit ordering over the window, aggregating functions keep what postgress calls a _running average_ of the value of the aggregating function. Let's take a look:

{% highlight sql %}
  select name, age, grade,
         avg(grade) over (partition by age order by grade)
  from students;
{% endhighlight %}

| name     |      age      |  grade| avg|
|----------|---------------|-------|-------|
| jack |  10 | 5 | 5.0 |
| john |    10   |  6 | 5.5 |
| nicole | 11 |    5 | 5.0 |
| susan | 11 |    6 | 5.5 |
| tracey | 11 |    7 | 6.0 |

<br>

The `avg` column now just takes the average of the records within the window _that it has already seen_; not the average of all the records in the window overall.

This gets especially confusing with the last value function. This is the behavior I encountered:

{% highlight sql %}
  select name, age, grade,
         last_value(grade) over (partition by age order by grade)
  from students;
{% endhighlight %}

| name     |      age      |  grade| last_value|
|----------|---------------|-------|-------|
| jack |  10 | 5 | 5 |
| john |    10   |  6 | 6 |
| nicole | 11 |    5 | 5 |
| susan | 11 |    6 | 6 |
| tracey | 11 |    7 | 7 |

<br>

The `last_value` function cannot see beyond the current row, so it returns the last value it has seen thus far. This is extremely counter-intuitive - you would think that asking for the `last_value` over a window would give you the last value of that column in the window. In this case, we might expect the `last_value` column to give the highest grade anyone with the current age is in. Unfortunately it does not.

As an aside, it's not quite true that these aggregating functions cannot see past the current row. They actually cannot see past the current row's _peers_, where a peer is a record in the same window that has the same ordering as the current row.

How do we get around this? Well, if you don't include the `order by` clause, the aggregating function applies over the whole window.

What if you need to order the partition? From postgres' documentation:

> To obtain aggregation over the whole partition, omit ORDER BY or use ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING.

This really just tells the query to always consider the full partition when it applies a window function, which leaves us with the following:

{% highlight sql %}
  select name, age, grade,
         last_value(grade) over (partition by age order by grade rows between unbounded preceding and unbounded following)
  from students;
{% endhighlight %}

| name     |      age      |  grade| last_value|
|----------|---------------|-------|-------|
| jack |  10 | 5 | 6 |
| john |    10   |  6 | 6 |
| nicole | 11 |    5 | 7 |
| susan | 11 |    6 | 7 |
| tracey | 11 |    7 | 7 |

<br>

And that's how to apply an aggregating function to a full ordered partition in postgres.
