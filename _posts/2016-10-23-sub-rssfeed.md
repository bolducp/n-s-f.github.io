---
layout: post
title: How to create a sub-RSS feed in Jekyll
date: 2016-10-23
---

It is sometimes useful to split up your RSS feed by topic, especially if you post on a wide variety of topics. Someone interested in your posts about programming might be less interested in your political musings. If you're running a business site on Jekyll, this becomes even more relevant. Splitting up your feeds would enable people to subscribe to only the posts they care about.

This post will demonstrate one way to split your rss feed into subfeeds using tags.

### Creating the feed

If you are already running a Jekyll site, there should already be a `feed.xml` in the base directory of your project. This represnts the RSS feed for all your posts.

Let's say you want to create a sub-feed for all your political content. Copy your `feed.xml` file over to a `politics-feed.xml` file, also in the base directory of your project.

Add an `if` statement immediately inside of the `for` loop that iterates over the posts, and use it to check whether a give post is tagged with the `politics` tag.

Once you've done this, your new `politics-feed.xml` will look like this:

{% highlight xml %}{% raw %}
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>{{ site.title | xml_escape }}</title>
    <description>{{ site.description | xml_escape }}</description>
    <link>{{ site.url }}{{ site.baseurl }}/</link>
    <atom:link href="{{ "/feed.xml" | prepend: site.baseurl | prepend: site.url }}" rel="self" type="application/rss+xml"/>
    <pubDate>{{ site.time | date_to_rfc822 }}</pubDate>
    <lastBuildDate>{{ site.time | date_to_rfc822 }}</lastBuildDate>
    <generator>Jekyll v{{ jekyll.version }}</generator>
    {% for post in site.posts limit:10 %}
----> {% if post.tags contains "politics" %}
        <item>
            <title>{{ post.title | xml_escape }}</title>
            <description>{{ post.content | xml_escape }}</description>
            <pubDate>{{ post.date | date_to_rfc822 }}</pubDate>
            <link>{{ post.url | prepend: site.baseurl | prepend: site.url }}</link>
            <guid isPermaLink="true">{{ post.url | prepend: site.baseurl | prepend: site.url }}</guid>
            {% for tag in post.tags %}
            <category>{{ tag | xml_escape }}</category>
            {% endfor %}
            {% for cat in post.categories %}
            <category>{{ cat | xml_escape }}</category>
            {% endfor %}
        </item>
----> {% endif %}
    {% endfor %}
  </channel>
</rss>
{% endraw %}{% endhighlight %}

### Adding tags

Now all that's left is to add the `politics` tag to your posts frontmatter. All this requires is adding a `tags` section to your frontmatter, or, if you already have one, simply adding the politics tag.

{% highlight yaml %}
---
layout: post
title: Interesting political idea
tags: politics
date: 2016-10-23
---
{% endhighlight %}

And that should do it! Your subfeed will now be available for subscription at http://_yourblog_.com/politics-feed.xml.

You can create as many sub-feeds in this way as you'd like, although if you have more than one or two you might want to consider moving them all into a `feeds` subdirectory.
