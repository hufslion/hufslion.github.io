---
layout: post      # 수정 NO 
title: 6. 게시판만들기(CR), admin페이지 활용하기 # 글 제목으로 수정
category: 7 # 본인에게 맞는 카테고리명을 적으세요
---

세션 6 - prod. 장윤서 

# 6th session

## contents
1. model 설정하기
  * class로 모델 설정
2. admin 페이지 활용하기 
  * admin 계정 만들기 
  * admin 페이지 살펴보기
3. home (전체 글 보이는 메인 페이지) 만들기 (Read)
  * Queryset
4. detail(특정 한 게시물 보는 페이지) 만들기 (Read)
  * pk, path converter, get_object_or_404
5. new(글쓰는 페이지) 만들기 (Create)
  * method-Post


### 본격적 코딩!!

가장 먼저 파일을 하나 생성하고 그 파일안에 들어간 다음 가상환경 파일을 만들어봅시다 

{% highlight html %}
python -m venv myvenv // 가상환경 파일 설치
{% endhighlight %}

그후 가상환경을 키고 django 설치까지 해봅시다

{% highlight html %}
source myvenv/Scripts/activate // 가상환경 키기 
pip install django // djnago 설치
{% endhighlight %}

django 설치가 완료 됬으면 이제 project 와 app을 만들어봅시다

{% highlight html %}
Django-admin startproject postproject// project 생성
cd postproject // postproject 파일에 들어가기
python manage.py startapp blogapp // app 생성
{% endhighlight %}

그후 postproject\postproject\settings.py에 들어가서 우리가 만들어준 app을 명시해줍시다

{% highlight html %}
	INSTALLED_APPS = [
	'django.contrib.admin',
	'django.contrib.auth',
	'django.contrib.contenttypes',
	'django.contrib.sessions',
	'django.contrib.messages',
	'django.contrib.staticfiles',
	'blogapp.apps.BlogappConfig', 
	//앱이름.apps.첫글자대문자 앱이름Config
	]
{% endhighlight %}

### 모델 설정하기
이제 postproject\blogapp\models.py에 들어가서 우리가 어떤 형식의 글을 쓸 것인지 말해줍시다 

{% highlight html %}
class Blog(models.Model):
  // 클래스 명을 적을땐 항상 대문자!!!
      title = models.CharField(max_length=100)
  // title 이라는 속성은 문자열 최대 길이가 100
      pub_date = models.DateTimeField('date published')
  // pub_date 라는 속성은 날짜, 시간
      body = models.TextField()
	// body라는 속성은 긴 물자열 	
      def __str__(self):
	  return self.title
  // 추후에 home 화면에서 각 글 번호가 아닌 제목으로 나타나도록 설정
{% endhighlight %}

이렇게 모델을 설정해줬으면 모델은 다른 파일들과 다르게 수정 했을 때 마다
터미널에 추가적으로 명령어를 입력해야 변경사항이 적용됩니다
이때 우리가 사용하는 DB(Data Base)는 django python과 별개이므로      
우리가 python으로 짠 코드를 받아드리도록 makemigrations 를 입력하고
migrate 통해 그 받은 내용을 적용하라고 명령어를 쳐줘야합니다 

{% highlight html %}
python manage.py makemigrations
//migration을 만들기

python manage.py migrate
//수정한 모델의 내용을 DB에 적용
{% endhighlight %}

### admin 페이지 활용하기
이제 모델이 잘 적용됬는지 확인하기 위해서 서버를 키고 admin 페이지를 통해 글을 써봅시다

먼저 postproject\blogapp\admin.py에 들어가서 admin에 Blog 모델을 사용한다는걸 명시해줍시다 
{% highlight html %}
from .models import Blog
//같은 파일에 있는 model내에 있는 Blog class 를 불러온다
admin.site.register(Blog)
//admin 사이트에 Blog class를 등록한다
{% endhighlight %}

이제 서버를 돌리고 

{% highlight html %}
python manage.py runserver
{% endhighlight %}

127.0.0.1/admin 에 접속하는 다음과 같이 로그인하라고 화면이 나옵니다 
![admin페이지 로그인화면](https://user-images.githubusercontent.com/26545947/55047950-c9545680-5089-11e9-9d56-eb23b1fbb6aa.png)
이때 아직 우리는 admin 계정이 없으므로 다시 터미널에 가서 admin 계정을 만드는 명령어를 칩시다
명령어를 치고 아이디와 비번을 입력해줍시다(이때 비번은 쳤을때 터미널 상으론 아무것도 안나옵니다!)

{% highlight html %}
python manage.py createsuperuser
{% endhighlight %}

admin 계정을 만든 후 로그인을 하면 다음과 같이 모델이 적용된 걸 볼 수 있습니다!
![admin페이지모델적용된화면](https://user-images.githubusercontent.com/26545947/55048007-f99bf500-5089-11e9-8848-64fdb48f53d9.png)

이제 blog의 add 버튼을 눌르면 다음과 같은 글쓰기 화면이 생깁니다 
![admin글쓰는화면](https://user-images.githubusercontent.com/26545947/55048017-01f43000-508a-11e9-9709-77f2bd8df7b8.png)

양식에 맞게 글을 써주고 save를 누르면 다음과 같이 글이 보입니다!
![admin글 목록 페이지](https://user-images.githubusercontent.com/26545947/55048030-0a4c6b00-508a-11e9-9f2b-0b209edd34f0.png)

### home(글 목록이 보이는 페이지) 만들기(Read)

이젠 이렇게 만들 글이 메인 페이지에 나오도록 해봅시다 
먼저 메인페이지로 할 home 을 만들어봅시다 
자신의 app 폴더 내에 templates 라는 폴더를 만들고 그 안에 home.html이라는 파일을 만들고 다음과 같이 코드를 쳐줍시다.

{% highlight html %}
<h1> 이곳은 Home 페이지 입니다 </h1>
{% endhighlight %}

이제 사이트에 들어오면 메인 사이트인 home이 바로 나오도록 views.py와 urls.py를 설정해줍시다 

먼저 postproject\blogapp\views.py에 들어가서 다음과 같이 코드를 쳐줍니다 

{% highlight html %}
def home(request): 
  // request(요청)만 들어오면 실행되는 함수
    return render(request, 'home.html')
  // 들어온 요청에 대해 home.html을 보여준다
{% endhighlight %}

이제 postproject\postproject\urls.py에 들어가서 다음과 같이 코드를 쳐줍니다

{% highlight html %}
import blogapp.views
//blogapp의 view에 있는 함수들을 끌어온다
	
urlpatterns = [
		path('admin/', admin.site.urls),
		path('', blogapp.views.home, name="home"),
    // 맨 처음 사이트에 들어오면, blogapp에 있는 views.py에 있는 home 함수를 실행한다, 이때 이 url 이름을 home이라고 한다
]
{% endhighlight %}

이제 다시 run server를 하고 들어가보면 다음과 같은 화면이 나옵니다! 
![home 화면](https://user-images.githubusercontent.com/26545947/55048040-16382d00-508a-11e9-8124-1b64ee36752d.png)

이제 home 화면이 나오도록 설정 했으니 home 화면에 쓴 글이 뜨도록 해봅시다!!

다시 postproject\blogapp\views.py에 들어가서 다음과 같이 코드를 쳐줍니다

{% highlight html %}
from .models import Blog
//같은 폴더 내에 있는 모델에서 Blog class를 가져온다
	
def home(request):
//요청이 들오면
    blogs = Blog.objects
//blogs 라는 변수 안에 모델로보터 받은 Blog의 객체(여기선 글 목록들)을 넣어 놓는다
//이때 Blog.objects 같은 모델로부터 받은 객체 목록을 "Queryset" 이라고 합니다
    return render(request, 'home.html', {'blogs': blogs})
// 들어온 요청에 대해서 home.html을 보여주고 있때 home.html(template)에서 위에 blogs라는 변수를 blogs라는 이름으로 가져올 것이다 
// {'blogs'= 템플릿에서 사용할 이름: blogs= view에서 설정한 변수 명}
   
{% endhighlight %}

다음은 이제 home.html로 가서 코드를 추가해줍시다 
{% highlight html %}
	 {%raw%}{% for blog in blogs.all %}{%endraw%}
      // views에서 설정한 글의 목록들이 담긴 blogs에서 담긴 모든 글을(blogs.all) blog로 각각 글을 하나씩 나눠 담고 
		<div class="container">
			{%raw%}<h1>{{ blog.title }}</h1>{%endraw%}
      // 그 나눠 담은 글의 제목
			{%raw%}<p>{{ blog.pub_date }}</p>{%endraw%}
      // 그 나눠 담은 글의 생성 시간
			{%raw%}<p>{{ blog.body }}</p>{%endraw%}
      // 그 나눠 담은 글의 내용
		</div>
 {%raw%}{% endfor %}{%endraw%}
{% endhighlight %}

이렇게 하고 다시 사이트에 들어가면 다음과 같이 글의 제목과 내용이 뜹니다! 
![글목록 뜨는 home화면](https://user-images.githubusercontent.com/26545947/55048045-1df7d180-508a-11e9-84f0-da9d25e4518b.png)

### 한 게시물이 보이는 페이지(detail) 만들기 (Read)

이제는 특정 한 게시물에 대한 페이지인 detail 페이지를 만들어 봅시다 
먼저 postproject\blogapp\templates 안에 detail.html 을 만들고 그 안에 다음과 같이 코드를 쳐줍시다 

{% highlight html %}
<h1> 디테일 페이지 입니당 </h1>
{% endhighlight %}

 그후 urls.py에 가서 다음과 같이 코드를 쳐줍시다 

{% highlight html %}
urlpatterns = [
     path('admin/', admin.site.urls),
     path('', blogapp.views.home, name="home"),
     path('blog/<int:blog_id>/', blogapp.views.detail, name='detail'),
     ]
{% endhighlight %}
여기서 저 <int:blog_id> 는 각 게시물의 id(게시물의 고유 번호 값)이 담기는 곳으로 path-converter라고도 합니다
이 path-converter 주로 url를 통해 views.py 의 함수에 특정 값이나 데이터를 넘겨줄때 사용합니다 

이제 저 path-converter인 <int:blog_id> 를 통해 특정 게시물의 id 값이 views의 detail 함수로 넘겨줬으니 
views.py로 가서 다음과 같이 detail 함수를 정의해봅시다 

{% highlight html %}
from django.shortcuts import render, get_object_or_404
//django에 내재된 get_object_or_404 기능을 가져온다
from .models import Blog
	
def home(request):
    blogs = Blog.objects
    return render(request, 'home.html', {'blogs': blogs})
		
def detail(request, blog_id): 
//요청과 url를 통해 blog_id 값을 전해 받고 
    details = get_object_or_404(Blog, pk=blog_id)
//Blog 모델에서 blog_id값을 pk로 삼아서 객체를 찾는다 없으면 404 error 를 담아라
    return render(request, 'detail.html', {'details':details})
// 요청에 대해서 detail.html를 보여주고 위의 details 변수를 template에 details변수에 담아서 보낸다
{% endhighlight %}

이때 저 pk라는건 primary key이며 모델에서 나오는 수많은 객체들을 구분하는 기준을 말하며 이번 코드에서는 blog_id값을 
통해서 구분지었습니다.

이제 한번 http://127.0.0.1:8000/blog/1/ 에 들어가보면 다음과 같이 detail 페이지가 뜨는걸 확인 할 수 있습니다!! 
![detail 초기 화면](https://user-images.githubusercontent.com/26545947/55048057-2c45ed80-508a-11e9-912e-89ac4fc5e962.png)

이젠 이 detail 페이지에 그 특정 한 게시물만의 제목, 내용 뜨도록 코드를 쳐봅시다 
먼저 detail 페이지에 다음과 같이 코드를 쳐주세요 

{% highlight html %}
{%raw%}<h1>{{details.title}}</h1>{%endraw%}
{%raw%}<p>{{details.pub_date}}</p>{%endraw%}
{%raw%}<p>{{details.body}}</p>{%endraw%}
{% endhighlight %}

이는 아까 views.py 의 detail 함수에서 detail.html 로 변수를 넘길때 detail 이라는 변수에 담아서 넘겼으므로 
이 detail 변수 안에 특정 한 게시물이 들어가 있으므로 이 게시물의 대한 제목, 생성 날짜, 내용을 나타내기 위해 
details.title  details.pub_date details.body 로 나타냈습니다 

이제 그럼 다음과 같은 화면이 나옵니다 

![detail 게시물 나오는 페이지](https://user-images.githubusercontent.com/26545947/55048063-349e2880-508a-11e9-966b-f66733bcc35d.png)

이제 메인 페이지인 home에서 특정 게시물의 제목을 누르면 그 게시물 페이지인 detail로 가도록 해봅시다 
home.html로 가서 다음과 같이 코드를 쳐주세요 

{% highlight html %}
 {%raw%}{% for blog in blogs.all %}
<div class="container">
	<a href="{% url 'detail' blog.id %}">
	<h1>{{ blog.title }}</h1>
	</a>
		<p>{{ blog.pub_date }}</p>
		<p>{{ blog.body }}</p>
</div>
 {% endfor %}{%endraw%}
{% endhighlight %}

제목에 해당하는 blog.title를 a 테그로 감싼 후 그 url은 아까 urls.py의 detail url로 연결하고 이때 이 a 테그를 눌렀을 때 그 url에 blog.id를 넘겨주도록 한다 라는 내용을 추가했습니다 
	
이제 다시 사이트에 가보면 home 화면에서 각 게시물의 제목에 링크가 달렸고 누르면 detail 화면으로 넘어가게 됩니다!!!!

이제 마지막으로 admin 페이지를 통해서가 아닌 우리 사이트의 글쓰는 페이지(new) 를 만들어서 글을 만들어 봅시다! 
먼저 templates 폴더 안에 new.html을 만들어주고 home 화면에서 글쓰러 가기 라는 링크를 누르면 글쓰는 페이지(new)로 가도록 template/ view/ url 을 설정해봅시다 

urls.py
{% highlight html %}
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', blogapp.views.home, name="home"),
    path('blog/<int:blog_id>/', blogapp.views.detail, name='detail'),
    path('blog/new/', blogapp.views.new, name='new'),
]
{% endhighlight %}

views.py
{% highlight html %}
def new(request):
    return render(request, 'new.html')
{% endhighlight %}

home.html
{% highlight html %}
{%raw%}{% for blog in blogs.all %}{%endraw%}
<div class="container">
    {%raw%}<a href="{% url 'detail' blog.id %}">{%endraw%}
        {%raw%}<h1>{{ blog.title }}</h1>{%endraw%}
    </a>
    {%raw%}<p>{{ blog.pub_date }}</p>{%endraw%}
    {%raw%}<p>{{ blog.body }}</p>{%endraw%}
</div>
{%raw%}{% endfor %}{%endraw%}
{%raw%}<a href="{% url 'new' %}"> new post </a>{%endraw%}
{% endhighlight %}

new.html
{% highlight html %}
	<div class="container">
		<form action="">
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
{% endhighlight %}

이렇게 코드를 치고 난 뒤 home 의 new post 링크를 누르면 다음과 같은 글쓰는 양식이 나와있는 new 페이지가 
나오게 됩니다 
![new 페이지 ](https://user-images.githubusercontent.com/26545947/55048073-42ec4480-508a-11e9-8511-e2d9bd63e803.png)


하지만 이상태로 글을 쓴다음 제출하기를 눌러도 글이 써지지 않습니다. 
왜냐하면 글을 쓰면 그 쓴 정보가 저장되도록하는 함수가 없기 때문입니다 
이제 그럼 글이 저장이 되도록 또 다시 template/ views.py/ urls.py를 설정해봅시다 

new.html
{% highlight html %}
<div class="container">
    {%raw%}<form action="{% url 'create' %}" method="POST">{%endraw%}
    /form 입력을 하면 'create'라는 이름의 url을 실행하는데 이때 POST 방식으로 데이터를 넘긴다
    {%raw%}{% csrf_token%}{%endraw%} 
    /POST method 를 사용할때 이 데이터는 신뢰값이 있는 데이터임을 설정해주는 토큰
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
{% endhighlight %}

views.py
{% highlight html %}
from django.shortcuts import render, get_object_or_404, redirect
// django에 내재 되어 있는 redirect 라는 기능을 가져온다
from django.utils import timezone
// django.utils 에서 timezone 기능을 가져온다
from .models import Blog
	
def home(request):
    blogs = Blog.objects
    return render(request, 'home.html', {'blogs': blogs})

def detail(request, blog_id): 
	details = get_object_or_404(Blog, pk=blog_id)
	return render(request, 'detail.html', {'details':details})

def new(request):
    return render(request, 'new.html')

def create(request):
//요청이 들어오면 
    blog = Blog()
//Blog 모델에서 빈 객체를 가져와서 blog라는 변수에 집어 넣고
    blog.title = request.POST['title']
//blog 변수에 담긴 빈 객체의 title 속성 값 = POST 방식으로 전달 받은 title 값
    blog.body = request.POST['body']
//blog 변수에 담긴 빈 객체의 body 속성 값 = POST 방식으로 전달 받은 body 값
    blog.pub_date = timezone.datetime.now()
//blog 변수에 담긴 빈 객체의 pub_date 속성 값 = 현재 시간
    blog.save()
//blog 변수에 담긴 값을 저장
    return redirect('/blog/'+str(blog.id)) 
//위에 저장까지 끝나면 바로 /blog/게시물id url로 넘어가라
{% endhighlight %}

urls.py
{% highlight html %}
urlpatterns = [
	path('admin/', admin.site.urls),
	path('', blogapp.views.home, name="home"),
        path('blog/<int:blog_id>/', blogapp.views.detail, name='detail'),
        path('blog/new/', blogapp.views.new, name='new'),
        path('blog/create/', blogapp.views.create, name='create'),
]
{% endhighlight %}

이후 아까와 똑같이 글을 쓰고 제출하기를 하면 글이 써집니다!!!!     
오늘은 여기까지! 수고하셨습니다!!!

### 피피티
[session6ppt](https://github.com/hufslion/for-pic-url/files/3013352/session6.pdf)
