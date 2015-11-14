# Jekyll Category Pagination

Simple plugin to generate pages for specific categories.
Categories has to be defined in your `_config.yml`

## Parameters

- 'pagination' how many posts per page
- 'layout' which layout to use
- 'directory' target directory of generated pages
- 'details' list of categories to generate pages for, each one can have
  title, layout, pagination and weight (which can be used within your navigation template
  for sorting)

## Template Data

- 'page.posts' list of posts
- 'page.nextUrl' url to next page, only available if next page exists
- 'page.prevUrl' url to previous page, only available if previous page exists
- 'page.posts' number of posts
- 'page.category' your category
- 'page.weight' your weight
- 'page.title' your title
- 'page.number' the current page number
- 'page.more' boolean, true if there is one more site

## Example Configuration

Add the following example to your `_config.yml`, the details map should
contain all categories you want to generate. The example would generate
the categories 'cat1', 'cat2' and 'cat3'.

``` yml
  categories:
    pagination: 5
    directory: categories
    details:
      cat1:
        title: Cat1
        weight: 25
      cat2:
        title: Cat2
        weight: 50
      cat3:
        title: Cat3
        weight: 75
```

## Example Template

Add the following template to your `_layout` folder.

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
