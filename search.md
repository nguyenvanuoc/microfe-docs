---
layout: post
title: Tìm kiếm
permalink: /search/
---

<form action="{{ site.baseurl }}/search/" class="search-box">
    <input type="text" class="form-control" placeholder="Nhập từ khóa tìm kiếm" id="search-box" name="query" autofocus>
    <span class="icon isax-search-normal-11"></span>
    <input type="submit" value="Search" style="display: none;">
</form>
<ul id="search-results"></ul>
<script>
  window.store = {
    {% for post in site.posts %}
      "{{ post.url | slugify }}": {
        "title": "{{ post.title | xml_escape }}",
        "author": "{{ post.author | xml_escape }}",
        "category": "{{ post.category | xml_escape }}",
        "content": {{ post.content | strip_html | strip_newlines | jsonify }},
        "url": "{{ post.url | xml_escape }}"
      }
      {% unless forloop.last %},{% endunless %}
    {% endfor %}
  };
</script>
<script src="{{ site.baseurl }}/scripts/lunr.min.js"></script>
<script src="{{ site.baseurl }}/scripts/search.js"></script>
