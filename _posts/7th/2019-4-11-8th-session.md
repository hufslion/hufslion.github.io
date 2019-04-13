---
layout: post
title: 08. 템플릿 상속 및 url 관리.
category: 7th
---
세션 8 - prod. 승윤

---

[예제 파일](https://github.com/seungyuns/newbitonproject/files/3023906/postproject.zip)

## 템플릿 상속 이란?

* 이번 강의에서는 저번 강의까지 만든 블로그페이지 코드를, 템플릿 상속과 url관리를 통해 보다 더 효율적으로 만드는 방법을 진행하도록 하겠습니다.

* 웹서비스를 사용하다 보면, 각 페이지마다 공통적으로 존재하는 요소들이 있는데요, 예를 들면 페이지 상단에 위치한 메뉴바가 대표적입니다.
이러한 공통적인 요소들을 각 페이지의 html 파일마다 똑같이 적어준다고 생각하면, 굉장히 비효율적이라는 것을 쉽게 알 수 있습니다. 이럴때 사용할 수 있는 것이 바로 템플릿 상속입니다.

* 템플릿 상속을 쉽게 설명해 보자면, 공통된 요소들을 한 html파일에 모두 담아두고, 각 페이지에 필요할 때마다 불러와서 쓰는 방법이라고 생각하면 될 것 같습니다.

* 바로 실습으로 넘어가도록 하겠습니다. 모두 예제파일을 다운받아 주시고, VS로 폴더를 열어주시기 바랍니다.

## 템플릿 상속 실습.

* 항상 외부파일을 불러왔을때는 모델이 적용되지 않은 상태이기 때문에, 모델을 등록을 해 주는것이 좋습니다.(현재 작업 경로 먼저 설정해주기)
{% highlight html %}
python manage.py makemigrations
python manage.py migrate
{% endhighlight %}

* 먼저 공통된 코드들이 들어갈 html파일을 만들건데요, 프로젝트파일안에 templates폴더를 만들고 그안에 공통 코드가 들어갈 base.html이름의 파일을 만들도록 하겠습니다.(주의, 폴더이름과 프로젝트 이름이 같을 최상단 폴더를 프로젝트 폴더로 착각 할 수 있다.)

* 그리고 우리가 만든 공통 html 파일의 위치를 settings파일에 등록을 해줘야하는데요, 프로젝트 폴더 안 setting.py 파일에 들어가서 templates 경로를 등록해주도록 하겠습니다. 
{% highlight html %}
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': ['postproject/templates'],   // 이 부분에 프로젝트폴더이름/templates 경로를 등록
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
{% endhighlight %}

* 등록이 끝났으면 이제 base.html파일에 공통으로 넣을 요소들을 코딩해보도록 하겠습니다. 먼저 우리 blog 홈페이지가 굉장히 썰렁하기 때문에 부트스트랩 CDN을 활용하여 메뉴바를 만들어 보도록 하겠습니다.
* 부트스트랩 CDN 간략 소개 ( 부트스트랩 CDN을 쉽게 설명하자면, 부트스트랩이 이미 만들어 놓은 CSS, JS 파일 등을 인터넷으로 끌어와 스타일을 입힌다고 생각하시면 됩니다.) 
* 부트스트랩 CDN을 페이지로 불러오면, 부트스트랩 홈페이지에 올라와있는 다양한 요소들을 불러와 사용 할 수 있습니다.
* 먼저 부트스트랩 공식 홈페이지에 접속하여 CDN 을 복사하여 base.html 상단에 넣도록 하겠습니다. 

{% highlight html %}
<head>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
</head>
{% endhighlight %}

* 부트스트랩 components 에 있는 메뉴바(Navbar)에서 조금 내려가서 검색창이 없는 가장 간단한 메뉴바를 끌어오겠습니다.
* 이렇게 불러온 요소들은 제가 다양한 스타일을 입힐수도 있고 원하는대로 변형시킬 수도 있습니다. 이번에는 한번 검은색 메뉴바를 사용해보도록 하겠습니다.

{% highlight html %}
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="#">Navbar</a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNav">
    <ul class="navbar-nav">
      <li class="nav-item active">
        <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Features</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Pricing</a>
      </li>
      <li class="nav-item">
        <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
      </li>
    </ul>
  </div>
</nav>
{% endhighlight %}

* base.html에 공통으로 들어갈 코드를 작성하고 나면, 개별적인 페이지 코드들이 들어갈 부분들을 명시해 줘야하는데요, 우리는 blog 글들을 메뉴바의 밑에 표시할 것이기 때문에, 메뉴바 밑에 자리를 명시하도록 하겠습니다.

{% highlight html %}
  {%raw%}{%block contents%}{%endraw%}
  {%raw%}{%endblock%}{%endraw%}
{% endhighlight %}

* 이제 우리 blog홈페이지에서 저희가 만든 공통 html 파일을 불러올건데요, home.html 상단에 태그를 통해서 base.html을 불러오도록 하겠습니다.
{% highlight html %}
  {%raw%}{% extends 'base.html' %}{%endraw%}
{% endhighlight %}

* 그리고 이전에 base.html 에 block 태그로 나타날 내용을 명시해 준 듯이, 그 명시한 내용이 현재 페이지의 내용이라는 것을 명시해주기 위해,  block태그로 home.html 요소들을 묶어주도록 하겠습니다.

{% highlight html %}
{%raw%}{% block contents %}{%endraw%}
 // html. 내용
{%raw%}{% endblock%}{%endraw%}
{% endhighlight %}


* 이제 다시 한번 사이트에 접속을 해 보도록 하겠습니다. (홈페이지에 메뉴바가 정상적으로 떠야 함)

* 메뉴바가 정상적으로 뜨는게 보이시죠?, 그런데 이대로 보기에는 게시판이 너무 볼품이 없기 때문에 간단하게 부트스트랩위에 css작업을 해보겠습니다.(게시글이 가운데로 이동된 것은, div class가 container 인 요소에 대한 속성이 부트스트랩 css에 있기 때문에 자동으로 입혀진 겁니다.)       

* 먼저 간단하게 메뉴바 brand를 이름으로 바꾸고 간단하게 글짜 굵기, 크기, 색을 넣겠습니다, 링크도 home 페이지로 걸어두겠습니다.
{% highlight html %}
<a class="navbar-brand" href="{%raw%}{%url 'home'%}{%endraw%}" style="font-weight: 600; font-size:25px; color:orange; ">SeungYun</a>
{% endhighlight %}

* 다음 간단하게 부트스트랩 card 목록에서 components를 가져와서 홈페이지 게시글을 담아주겠습니다. for문 밑에 바로 삽입.

* 요소들을 카드에 옮겨주겠습니다. title 옮기고 a태그 옮기기, sub title에 pub_date날짜 옮기기, text 에 글 body 옮기기, 카드링크에 삭제하기 수정하기 옮기기.

* 게시글을 중앙으로 옮기기 위해 div container로 한번 더 감싸주고, card class의 style에 있는 넓이 속성을 제거해주도록 하겠습니다. 위에 공백하나도 어주기.

* new post 버튼도 부트스트랩 button component를 가져와서 넣어보도록 하고, 글씨가 잘 안보이니 글자색을 white로 변경하겠습니다.

{% highlight html %}
{%raw%}{% extends 'base.html' %}{%endraw%}
{%raw%}{% block contents %}{%endraw%}
{%raw%}{% for blog in blogs.all %}{%endraw%}
<br>
<div class="container">
    <div class="card" >
        <div class="card-body">
        <a href="{%raw%}{% url 'detail' blog.id %}{%endraw%}"> <h5 class="card-title">{{ blog.title }}</h5></a>
        <h6 class="card-subtitle mb-2 text-muted">{{ blog.pub_date }}</h6>
        <p class="card-text">{{ blog.body }}</p>
        <a href="{%raw%}{% url 'delete' blog.id%}{%endraw%}" class="card-link"> 삭제하기 </a>
        <a href="{%raw%}{% url 'update' blog.id%}{%endraw%}" class="card-link"> 수정하기 </a>
        </div>
    </div>
</div>
{%raw%}{% endfor %}{%endraw%}
<div class="container">
<button type="button" class="btn btn-dark"><a href="{%raw%}{% url 'new' %}{%endraw%}"  style="color:white;" > new post </a></button>
</div>
{%raw%}{% endblock%}{%endraw%}
{% endhighlight %}


* 이제 얼추 게시판 모양이 나타났습니다. 하지만, 아직 저희가 detail, update, new 페이지는 템플릿 상속을 안해주었기 때문에, 템플릿을 마저 상속시키도록 하겠습니다. 

* detail 페이지, home페이지 그대로 긁어오고, blog 인자 details로 바꿔주기. for문 end 문 지우기.
{% highlight html %}
{%raw%}{% extends 'base.html' %}{%endraw%}
{%raw%}{% block contents %}{%endraw%}
<br>
<div class="container">
    <div class="card" >
        <div class="card-body">
        <h5 class="card-title">{{details.title}}</h5>
        <h6 class="card-subtitle mb-2 text-muted">{{details.pub_date}}</h6>
        <p class="card-text">{{details.body}}</p>
        <a href="{%raw%}{% url 'delete' details.id%}{%endraw%}" class="card-link"> 삭제하기 </a>
        <a href="{%raw%}{% url 'update' details.id%}{%endraw%}" class="card-link"> 수정하기 </a>
        </div>
    </div>
</div>
{%raw%}{%endblock%}{%endraw%}
{% endhighlight %}

* 새글쓰기 창, base.html 불러오기, block contents 감싸주기, <br>한칸 뛰고, div class=container 로 한번 더 감싸주기.
{% highlight html %}
{%raw%}{% extends 'base.html' %}{%endraw%}
{%raw%}{% block contents %}{%endraw%}
<br>
<div class="container">
<div class="container">
    <form action="{%raw%}{% url 'create' %}{%endraw%}" method="POST">
    {%raw%}{% csrf_token%}{%endraw%} 
    <h4>제목: </h4>
    <input type="text" name="title">
    <br>
    <br>
    <h4>본문: </h4>
    <textarea cols=40 rows=10 name="body"></textarea>
    <br>
    <br>
    <input class="btn btn-dark" type="submit" value="제출하기">
</form>
</div>
</div>
{%raw%}{%endblock%}{%endraw%}{% endhighlight %}

* update창, base.html 불러오기, block contents 감싸주기, <br>한칸 뛰고, 수정페이지 h태그 div 안으로 옮겨주기. 글자 스타일 바꿔주기. 

* 이로서 blog project 모양이 어느정도 잡히게 되었습니다. 나중에 시간이 되시면 다양한 css속성을 활용해서 좀 더 이쁘게 게시판을 조정하시기 바랍니다.

## URL 관리의 필요성.

* 이제부터는 URL 관리에 대해서 배우도록 하겠습니다. 

* 현재 우리 프로젝트에는 게시판기능을 가지고있는 blogapp이라는 앱이 하나만 존재하는데요, 그렇기 때문에 프로젝트 폴더에 있는 urls.py 에서 모든 url을 관리해도 크게 불편함이 없습니다.
* 하지만, 프로젝트가 커지면 커질수록 다양한 앱이 생겨날 것이고, 생겨난 모든 앱의 url을 한개의 urls 파일에서 관리한다는 것은 비효율적이고 아주 복잡해 질 수 있습니다.
* 그렇기 때문에 지금부터는 프로젝트폴더에 있는 urls파일에서 관리하던 app의 url을, 각자의 app폴더에서 따로 관리를 하고, 프로젝트 폴더로 템플릿상속을 하듯 필요한 app의 url을 불러와서 쓰도록 해보겠습니다.

## URL 관리 실습.

* 지금 프로젝트 폴더에 있는 urls.py 파일을 보시면, blogapp과 관련된 url이 적혀있는것을 확인할 수 있는데, 이 url들을 blogapp폴더에서 따로 관리하도록 하고, 프로젝트 폴더 url로 불러와서 쓰도록 하겠습니다.

* 먼저 blogapp 폴더에 똑같이 urls.py 파일을 생성하고, 필요한 요소들을 임포트해오도록 하겠습니다. (주의 import blogapp.views --> from . import views(현재 폴더(blogapp)에 존재하는 views 파일을 임포트하겠다는 의미)).

* urlpatterns 리스트를 만들고, 그안에 필요한 blogapp 관련 url을 모두 가져 오기.

* blogapp안으로 들어왔기 때문에, 앞에 불필요한 blogapp명시들은 없애도록 하겠습니다. (url pattern blog 없애기, views앞에 명시 없애기).

* 이제 blogapp url파일을 프로젝트 url파일로 끌어올건데요, 이러한 과정을 도와줄 기능이 바로 django.urls에 있는 include 입니다. 프로젝트폴더 url파일에서 path와 함께 include를 임포트 해오도록 하겠습니다.

* 마지막으로, blogapp 폴더의 urls파일을 프로젝트폴더로 끌고오기 위해 경로를 추가해주도록 하겠습니다. 
{% highlight html %}
path('blog/', include('blogapp.urls')),
{% endhighlight %}
* 코드를 보시면 어느정도 느낌이 오실텐데, blogapp폴더의 urls 파일을 불러오고 그 url의 형식은 blog/ ... 뭐뭐뭐 로 하겠다 라는 의미 입니다.

* 이렇게 url을 각자의 app에서 관리한다면, 프로젝트가 커져도 효율적이고 체계적으로 url을 관리하실 수 있습니다.

## 번외 : 메뉴 active 기능 넣기.

* 이번에는 base.html에 작성한 메뉴바에 active 기능을 추가하는 방법을 알려드리겠습니다. 인터넷을 사용하다보면, 현재 들어온 메뉴의 글씨가 강조되는 것을 확인할수 있는데요, 이렇게 자신이 들어온 메뉴 부분을 강조하는 방법은 굉장히 다양하지만 그 중 가장 간단한 방법을 알려드리겠습니다. 

* 먼저 views.py의 home 함수에 인자를 하나 추가로 넘겨주도록 하겠습니다. 
{% highlight html %}
def home(request):
    blogs = Blog.objects
    return render(request, 'home.html', {'blogs': blogs, 'navbar':'home'})
{% endhighlight %}

* 평소에 저희는 주로 변수를 담아서 넘겨줬었는데요, 저기에서 navbar에 'home'은 단순히 navbar에 'home' 이라는 string 타입의 정보를 담아 보내준다고 생각하면 됩니다.  

* 다음 base.html navbar 코드에서 기존에 nav-item으로 되어있던 li 태그 클래스를 아래와 같이 변경해주겠습니다.
{% highlight html %}
<li class="{%raw%}{% if navbar == 'home' %} active {% endif %} {%endraw%}" >
    <a class="nav-link" href="{%raw%}{% url 'home' %}{%endraw%}">Home</a>
</li>
{% endhighlight %}

* class 에 있는 if문은, 만약 변수 navbar가 'home'이라면 li 리스트 태그의 클래스를 active로 지정해주라는 코드인데, 아까 views 파일에서 home 함수를 호출하면, 'home'을 담을 navbar변수가 넘어가도록 설정해 놓았기 때문에, home 화면을 띄우면 자동으로 li 리스트 태그의 클래스가 active로 지정되게 됩니다.(글로쓰면 복잡, 그냥 말로 설명). 그렇기 때문에 active class에 css style을 설정해 놓으면, home 화면에 갈때마다 css가 입혀진 home 메뉴가 나타나게 되는 원리입니다.


# PPT참고

[템플릿 상속 및 url 관리하기.pdf](https://github.com/seungyuns/newbitonproject/files/3025735/8th_Session_PPT.pdf)

