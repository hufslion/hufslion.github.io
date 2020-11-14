---
layout: default
---
<!-- 첫 화면에서 보이는 게시글들의 미리보기 내용을 커스텀하는 공간이다. -->
<div class="posts">
<p style="display: inline;"> 8기 세션은 운영진의 Github 과 notion을 활용하여 세션 진행</p>
  {% for post in site.categories.8th-session %}
    <article class="post">
    

<!-- change to display: inline two tag      <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>{{ post.date | date: "%B %e, %Y" }}</h1> -->
      <h1 style="display: inline;"><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1><p style="display: inline;"> - {{ post.date | date: "%B %e, %Y" }}</p>

      <div class="entry">
        {{ post.excerpt }}
      </div>
<!-- 
      <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a> -->
    </article>
  {% endfor %}
</div>
