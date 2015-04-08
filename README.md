# A Jekyll template for iTunes podcast feed
An easy way to add a podcast to your Jekyll-based blog and publish it on iTunes is to:

1. Adjust `_config.yml` to add some podcast-specific static data.
2. Mark all the posts that are related to podcast with a certain category (sa, `podcast`).
3. Add podcast-specific attributes to each post that is related to podcast.

## Prepare

### Put static data to config

You can either hard-code podcast-specific data into layout file, or put this data into site config. I personally prefer the second approach since it's scalable and allows us use that data in other different ways.

So, to your `_config.yml` add several properties, like:

```
# Misc
licence: CC-BY-CA
# Podcast
podcast:
  title: My Cool Podcast
  art: http://link/to/cover/art.png
  category: Education
  subcategories:
  - Training
  subtitle: The subtitle that shows in "Description" column on desktop iTunes
  summary: >
    A long long description of your podcast
    that a listener can read in Podcast View.
```

If you host a cover art image somewhere outside the current site, `podcast.art` would be useful; otherwise, it's unnecessary since the link can be obtained via `base_url` and `url` properties of site config.

### Use a category to filter posts

To add a new item to the podcast feed, you simply create a new post in `_posts` folder and then in the top section, among layout (which can be any) and post date, you should specify a category, like

```
category: podcast
```

But it's not enough. iTunes demands specific data to be present in the feed, like file size in bytes or duration in `hh:mm:ss` format. These properties must be added to post's config, too, so it basically looks like

```
---
layout: post
title:  "Hello, World"
date:   2015-01-01 12:00:00
categories: podcast
number: 1
duration: 15:00
length: 1065610
mp3: http://link/to/mp3
---
```

Few points to pay attention to:

* `number` isn't necessary and is used just for subscribers' convenience.
* `mp3` links to the `audio/mpeg` file which contains the episode of podcast, and in this example is considered to be hosted elsewhere outside current site.
* `title` and `date` are conventional to Jekyll and are used by feed layout template, too.
* `duration` and `length` are what you should calculate by hand (via file attributes).

After post's config session goes post's content which is used in the feed generation process, too.

## Create a page for the feed

Since each page in Jekyll is actually a post or a page that is processed using template, the former is needed. So what you should do is basically create a `feed.md` file in root of site's folder and make it be transformed using iTunes feed template. Also, provide the permalink:

```
---
layout: itunes-feed-layout
permalink: /podcast/feed/
---
```

Make sure you put a trailing slash at the end of permalink. Without that, Jekyll will throw an error each time you want to generate the site.

## Put template into layouts folder

Next step is to put the template into `_layouts` folder. Just copy and paste.

## Adjust layout to meet your needs

Finally, make sure the layout, which soon will generate RSS feed for iTunes, uses existing properties from site config and post attributes. You may want to change the language of podcast
(which is hardcoded `en-us`),
change category name in item enumeration, or mark your podcast as safe for all ages.