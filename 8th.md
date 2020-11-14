---
layout: default
---
<!-- 첫 화면에서 보이는 게시글들의 미리보기 내용을 커스텀하는 공간이다. -->
<div class="posts">
<p style="display: inline;">코로나로 인한 오프라인 세션 진행시 전원 마스크 착용 및 위생 규칙 준수, 전원 명부 작성</p>
<br>
  {% for post in site.categories.8th %}
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
