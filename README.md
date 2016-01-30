# Jekyll Category Pagination

Simple plugin to generate numbered pages for specific categories. Categories
has to be defined in your `_config.yml` and will be generated as single html
files like `categories/<category>/page<N>.html`.

You can read more about this plugin in "[Category Pagination in Jekyll](https://blog.codestack.de/2015/05/02/category-pagination-in-jekyll.html)".
Furthermore this plugin can be used to create infinite scrolling blogs in
Jekyll, read more about how to do this in "[SEO Friendly Infinite Scrolling with Jekyll](https://blog.codestack.de/2015/05/17/seo-friendly-infinite-scroll.html)".

This plugin will generate one folder for each configured category within the
main folder 'categories'. Each category folder contains
files like page1.html, page2.html, and so on. In addition to that, a virtual
category `all` is added, which contains all posts with pagination.

## Installation

Install `paginator.rb` in your `_plugins` directory, add some layout and change
your configuration as described below.

## Parameters

Set your config options in `_config.yml` within 'categories'. The category `all`
is reserved by pagination for all posts, redefine this with
`category_for_all_posts`. The following options can be used:

- `pagination` how many posts per page, default is 5
- `layout` which layout file from your `_layouts` folder to use,
  default is 'category.html'
- `directory` target directory of generated pages, use '.' to place them
  directly in `_site`
- `details` list of categories to generate pages for, each one can have
  `title`, `layout`, `pagination` and `weight` (which can be used within your
  navigation template for sorting)
- `generate_all` used to generate all categories or categories within `details`

## Template Data

Within your template you can use the following information. Templates has do be
placed in your `_layouts` folder and should be specified in your configuration.
For example `layouts/category.html` has to be `layout: category.html` beneath
the 'categories:' key in your `_config.yml` file.

- 'page.posts' list of posts
- 'page.nextUrl' url to next page, only available if next page exists
- 'page.prevUrl' url to previous page, only available if previous page exists
- 'page.category' your category
- 'page.weight' your weight
- 'page.title' your title
- 'page.number' the current page number
- 'page.pages' the maximum number of pages
- 'page.more' boolean, true if there is one more site

## Example Configuration

Add the following example to your `_config.yml`, the details map should
contain all categories you want to generate. The example would generate
the categories 'cat1', 'cat2' and 'cat3'.

``` yml
  categories:
    pagination: 5
    generate_all: false
    directory: categories
    details:
      all: {}
      cat1:
        title: Cat1
        pagination: 10
        weight: 25
      cat2:
        title: Cat2
        weight: 50
      cat3:
        title: Cat3
        weight: 75
```

## Example Template

Add the following template to your `_layouts` folder and specify this in
your configuration, for example `_layouts/category.html` has to be specified
as `layout: category.html` beneath the 'categories:' key.

``` html
---
layout: default
---
<div class="post-list">
    {% for post in page.posts %}
    <div class="post">
      <header class="post-header">
        <h1 class="post-title">
            <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
        </h1>
        <p class="post-meta">{{ post.date | date: "%b %-d, %Y" }}
            {% if post.meta %} &middot; {{ post.meta }}{% endif %}</p>
      </header>
      <article class="post-content">
        {{ post.excerpt | markdownify }}
      </article>
      <hr/>
    </div>
    {% endfor %}
    {% if page.nextUrl %}
    <a class="page-next" href="{{ page.nextUrl | prepend: site.baseurl }}">next</a>
    {% endif %}
</div>
```

## Navigation

Each page gets into the pages list which is used for navigation generation. To
avoid unintended entries, check for titles and page numbers. You can do
something like this:

``` html
<ul>
{% for page in pages %}
{% if page.title %}
{% if page.number == nil or page.number == 1 %}
<li>
<a href="{{ page.url | prepend: site.baseurl }}">{{ page.title }}</a>
</li>
{% endif %}
{% endif %}
{% endfor %}
</ul>
```
