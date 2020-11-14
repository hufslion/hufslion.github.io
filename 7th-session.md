---
layout: default
---
<!-- 첫 화면에서 보이는 게시글들의 미리보기 내용을 커스텀하는 공간이다. -->
<div class="posts">
<p style="display: inline;"> 7기 세션은 해당 깃헙페이지를 사용하여 세션을 진행함</p>
<br>
  {% for post in site.categories.7th-session %}
    <article class="post">
    
<br>
<!-- change to display: inline two tag      <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>{{ post.date | date: "%B %e, %Y" }}</h1> -->
      <h1 style="display: inline;"><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1><p style="display: inline;"> - {{ post.date | date: "%B %e, %Y" }}</p>

<!-- 
      <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a> -->
    </article>
  {% endfor %}
</div>
