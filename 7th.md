---
layout: default
---
<!-- 첫 화면에서 보이는 게시글들의 미리보기 내용을 커스텀하는 공간이다. -->
<div class="posts">
  {% for post in site.categories.7th %}
    <article class="post">
<br>
<!-- change to display: inline two tag      <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>{{ post.date | date: "%B %e, %Y" }}</h1> -->
      <h1 style="display: inline;"><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1><br><p style="display: inline; ">{{ post.date | date: "%Y년 %m월 %e일 " }}</p>


<!-- 
      <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a> -->
    </article>
  {% endfor %}
</div>
