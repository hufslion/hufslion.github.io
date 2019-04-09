---
layout: post
title: 09. CR 복습/Static, Media/Thubmnail
category: session
---
정규세션 09 - prod. 이태훈  


## Session Preview

* 본 세션 자료에서는 지난 세션에서 진행하였던 CR(Create, Read)에 템플릿 상속을 곁들인 FrontEnd를 추가하여 간단한 복습을 진행해 보겠습니다.

* 또한 본격적인 DataBase 활용인 Static, Media 처리 방식에 대해 알아보겠습니다.

* 마지막으로는 Media를 통해 Image Thumnail 만드는 방법을 배워보도록 하겠습니다. 

## CR Review (feat. Navbar)

* 먼저 새 프로젝트를 만들어 CRUD 패턴 중 CR에 대한 복습을 해보도록 하겠습니다. 이제 다들 새 프로젝트를 시작하는 법에 대해선 도가 텄으실 것이라 믿고, project를 만들고 하위 app을 settings.py에 등록하는 등의 만성적 절차에 대해선 설명을 생략하도록 하겠습니다.

* 아, 그리고 시작하기 전에 앞서 가상환경 켜는 것을 꼭 잊지 말도록 합시다!

{% highlight html %}
	django-admin startproject 9th_session
	cd 9th_session
	python manage.py startapp board
{% endhighlight %}

**꼭 가상환경 실행을 잊지 맙시다! (source myvenv/Scripts/activate)**
*  App의 이름은 자유롭게 하셔도 무방합니다. 이번 CR 복습에서 만들 App은 간단한 쓰기와 읽기 기능이 있는 게시판 App이므로 저는 board라 지었습니다.
*  이제 App을 생성했으니 settings.py 에서 App과 프로젝트를 연결해줍시다.

{% highlight html %}
#Settings.py 내부
INSTALLED_APPS = [
'django.contrib.admin',
'django.contrib.auth',
'django.contrib.contenttypes',
'django.contrib.sessions',
'django.contrib.messages',
'django.contrib.staticfiles',
'board.apps.BoardConfig',
]
{% endhighlight %}

* 이제 templates 폴더를 App의 하위 폴더에 생성해줍시다.  

![templates_make](https://user-images.githubusercontent.com/46686577/55428086-5a17be80-55c3-11e9-8432-accda8f18919.png)

* templates 폴더 내부에 총 4개의 html 파일을 작성해줍시다. 이번 CR 복습에서는 템플릿 상속 기능을 활용해 볼 것이므로 기존의 3개에 base.html을 추가로 작성해줍니다.
  **따라서 base.html, home.html, detail.html, new.html 총 4개의 파일을 생성해 줍니다.**

* 이제 우리가 만든 html 파일을 서버에서 반환해줄 수 있도록 views.py 파일에서 반환 함수를 작성해보도록 합시다.

{% highlight html %}
def home(request):
return render(request, 'home.html')
{% endhighlight %}

* 이제 urls.py 에서 우리의 서버로 접속할 URL을 설정해주도록 합시다.

{% highlight html %}
import board.views #board APP의 views 파일을 가져옵니다.
urlpatterns = [
	path('admin/', admin.site.urls),
	path('', board.views.home, name="home"), #views 파일 내에 home 함수에서 반환받습니다.
]
{% endhighlight %}

* 이제 기본적인 세팅은 모두 끝났습니다.  이제 지난 세션에서 배웠던 템플릿 상속을 활용해보도록 합시다. 이번에는 부트스트랩을 사용한 간단한 디자인을 찹찹 뿌려줄 예정이므로 먼저 base.html을 설정해주어야 합니다. base.html을 열어주세요.

{% highlight html %}
{% raw %}
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport"  content="width=device-width, initial-scale=1.0">
	<meta http-equiv="X-UA-Compatible"  content="ie=edge">
	<title>Board</title>
	<link rel="stylesheet"  href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"  integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T"  crossorigin="anonymous">
	<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js"  integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo"  crossorigin="anonymous"></script>
	<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1"  crossorigin="anonymous"></script>
	<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM"  crossorigin="anonymous"></script>
</head>
<body>
	<nav class="navbar navbar-dark bg-dark">
		<div class="container">
			<a class="navbar-brand"  href="{% url 'home' %}">게시판</a>
			<a class="nav-link"  href="{% url 'new' %}">글쓰기</a>
		</div>
	</nav>
	{% block body %}
	{% endblock %}
</body>
</html>
{% endraw %}
{% endhighlight %}


* 이렇게가 base.html에서 작성해야하는 코드입니다. 코드가 길다고 무서워하지 마세요. head 태그 내부 부분은 단순히 부트스트랩 CDN을 끌어온 부분입니다.  

![bootstrap_cdn](https://user-images.githubusercontent.com/46686577/55428075-584dfb00-55c3-11e9-81ad-d6415b7990c7.png)

* body 내부의 nav 태그 부분은 부트스트랩의 컴포넌트 중 하나인 네비게이션 바를 적용시킨 겁니다. 이 부분은 부트스트랩의 Documentation에서 기술한 코드가 조금 너무 무거워보여서 제가 미니미하게 수정한 것이므로 편하신 분은 부트스트랩의 정규 예시 코드를 사용하셔도 좋습니다.

* 템플릿 상속의 핵심은 {% raw %} {% block body %}, {% endblock %} {% endraw %} 부분입니다. 이 부분으로 자식 템플릿 파일(html)들의 코드가 붙여넣기 될 것이고 이외의 상기 코드 부분은 모두 공유하게 됩니다. 보다 자세한 설명은 지난 세션을 참고해주세요.

**글쓰기 링크의 url 'new' 부분은 이후 Create 부분에서 다루게 될 부분입니다. 지금은 미리 작성해주세요.**

* 이제 우리의 메인 페이지인 home.html을 작성해봅시다. home.html을 열어주세요.

{% highlight html %}
{% raw %}

{% extends 'base.html' %}
{% block body %}
<div class="container">
	<h1>Posting List</h1>
</div>
{% endblock %}

{% endraw %}
{% endhighlight %}

* 이렇게 작성해 줍시다. {% raw %} {% extends 'base.html' %} {% endraw %} 부분이 바로 base.html을 상속받겠다, 즉, 나 자신(home.html)은 base.html의 자식 템플릿이 되겠다는 것을 의미합니다. 

* 이후 base.html 내에 지정해 주었던 body block에 붙여넣기 될 코드를 작성해주는 구문이 바로

{% raw %}

{% block body %}
---이하 코드---
{% endblock %}

{% endraw %}
입니다.

* 여기까지 작성하셨다면 이제 터미널에서 로컬 서버를 작동시켜 페이지를 확인해봅시다.

{% highlight html %}
	python manage.py runserver
{% endhighlight %}

![init_board](https://user-images.githubusercontent.com/46686577/55428077-584dfb00-55c3-11e9-8f3a-16979334cdec.png)

* 이 화면이 뜨고 계신다면 잘 하고 계신 겁니다. 이제 조금 더 깊숙한 부분으로 들어가 봅시다. models.py, 즉 DB에 등록할 데이터 형태를 주물러 볼 겁니다.

* 일단 models.py를 열어서 다음과 같이 작성해주세요.

{% highlight html %}
from django.db import models

class Posting(models.Model):
	title = models.CharField(max_length=100)
	pub_date = models.DateTimeField("date_published")
	body = models.TextField()

	def __str__(self):
		return  self.title
{% endhighlight %}

* 간단히 작성한 코드에 대해서 알아봅시다. 먼저 모델이란 무엇일까요? 모델이란 DB에 등록할 데이터의 형태를 추상화한 틀이라고 보시면 될 것 같습니다. Django 에서는 모델로 묶어낼 각종 데이터 타입을 django.db 속 models 모듈에 정리해 놓았습니다.

* 이제 모델의 형태, 즉 class를 볼까요? 여기서 클래스의 이름은 임의대로 명명하셔도 좋습니다. (다만 이 경우 이후 코드에서도 잘 수정해주셔야 합니다.) Model 클래스의 경우 인자로 import 해 온 models.Model을 넣어줍니다. 이럼으로써 이 클래스는 Model을 구성하는 틀로써 기능합니다. 이제 이 클래스의 구성 요소를 봅시다.

* models에는 여러 속성들이 있습니다. 그 중 CharField는 짧은 길이의 Text를, DateTimeField는 날짜 데이터를, TextField는 긴 길이의 Text 값들을 저장할 수 있는 속성들입니다. 이번 App에서는 게시판 기능을 구현해 볼 것이기 때문에 글의 제목, 글의 게시일, 글 본문, 총 3가지를 구성할 수 있도록 위의 3가지에 해당 속성들을 models에서 불러와 클래스의 변수로 넣어주었습니다.

*__str__ 함수는 해당 클래스로 생성된 인스턴스를 문자열화 하였을 때 반환되는 값을 지정해주는 함수입니다. 간단하게 말하자면 각각의 객체에 대해 이름을 붙이고자 할 때 이 함수를 써서 지정해준다고 생각해주시면 될 것 같습니다.*

* 이제 모델을 등록해 주었으니 DB에 모델이 등록된 것을 알려줍시다.
  **(!) 잊지마세요, 모델은 그저 DB에 등록할 틀을 만드는 것일 뿐입니다. 실제 DB는 SQLite3이며 이것은 장고와 별개의 것입니다. (!)**

* 터미널에 다음과 같이 입력해주면 장고가 DB에 "야, 새로 데이터 형태 등록해 놨어. 확인해봐!" 하고 알려주게 됩니다.

{% highlight html %}
	python manage.py makemigrations
	python manage.py migrate
{% endhighlight %}

* 실제로 App 내부를 확인해보시면 migrations 폴더 내에 테이블이 생긴 것을 확인하실 수 있을 겁니다.

![migrations](https://user-images.githubusercontent.com/46686577/55428079-58e69180-55c3-11e9-9c14-ca7fd93c0dae.png)

* 이제 우리가 해당 모델 데이터를 admin 레벨에서 관리할 수 있도록 admin.py에 모델을 등록하고 admin 계정을 생성해서 실제로 DB에 우리가 등록한 모델로 테이블이 만들어 졌는지 확인해봅시다.

* 먼저 admin 계정을 생성해줍시다. 터미널에 다음과 같이 쳐주세요.

{% highlight html %}
	python manage.py createsuperuser
{% endhighlight %}

<blockquote>
> 이 밑으로 ID와 PWD, Email 등을 입력하는 절차는 알아서 잘 하실 것이라 믿고 생략합니다.
</blockquote>

* 이제 admin.py에 가서 다음과 같이 코드를 작성하여 모델을 admin 레벨에 등록해줍니다.

{% highlight html %}
from django.contrib import admin
from .models import Posting

# Register your models here.
admin.site.register(Posting)
{% endhighlight %}

<blockquote>
> python manage.py runserver를 터미널에 쳐서 로컬 서버를 켠 뒤
> 주소 뒤에 /admin/을 입력해서 admin 페이지에 들어갑니다.
> admin 계정으로 로그인 해주면 다음과 같이 나와야 합니다.
</blockquote>

![admin](https://user-images.githubusercontent.com/46686577/55428091-5ab05500-55c3-11e9-915e-7cb829e1aa18.png)

* 모델이 등록된 것을 확인했으니 views.py에서 DB에 등록된 객체들을 실제로 템플릿에 뿌려줄 수 있도록 함수를 작성해 봅시다. 일단 만들어 둔 모델을 views.py로 끌어와야 합니다. 다음과 같이 코드를 작성해봅시다.

{% highlight html %}
from django.shortcuts import render
from .models import Posting

# Create your views here.
def home(request):
	postings = Posting.objects
	content = {'postings' : postings}
	return render(request, 'home.html', content)
{% endhighlight %}

* Posting 모델로 등록되어 있는 모든 객체를 .objects로 불러와 이를 Key-value 형태로 템플릿에 전달해주는 모습입니다.
* 반환 함수도 만들었으니 이제 실제로 템플릿에서 DB 객체를 받아서 뿌려질 수 있도록 코드를 다듬어 봅시다. home.html에서 다음과 같이 작성해주세요.

{% highlight html %}
{% raw %}
{% extends 'base.html' %}
{% block body %}
<div  class="container">
	<h1>Posting List</h1>
	{% for posting in postings.all %}
		<div  class="container">
			<h1>{{posting.title}}</h1>
			<p>{{posting.pub_date}}</p>
			<p>{{posting.body}}</p>
		</div>
	{% endfor %}
</div>
{% endblock %}
{% endraw %}
{% endhighlight %}

* 이제 admin 페이지에서 Posting 객체를 하나 만들어 본 뒤 로컬 서버를 작동시켜 home.html에 제대로 뿌려지고 있는지 확인해봅시다. 다음과 같이 나오면 잘 하신 겁니다.

![First_Cr](https://user-images.githubusercontent.com/46686577/55428076-584dfb00-55c3-11e9-8ddf-f3afd8ea789d.png)

* 이제 본격적으로 CR에 들어가봅시다. CR 중 Create부터 해볼 겁니다. 새로 '글을 쓰겠다'라는 요청(request)가 들어오면 views.py에서 새로운 DB 객체(세션 코드 기준으로 Posting 객체)를 만들어 반환하는 형태로 구성해봅시다.

* 새로 글을 쓰는 것은 new.html 템플릿을 이용할 겁니다. 이 때 주의해야 하는 점이 있습니다. 저희는 '새로 글을 쓰겠다'는 request와 단순히 '글 쓰는 페이지를 보여줘'라는 request를 구분할 방법이 없습니다. 따라서 글을 쓰는 request를 처리하는 함수는 create라는 함수로, 글 쓰는 페이지를 요구하는 request는 new라는 함수로 분리하여 개별 작성할 겁니다.

* 우선 views.py에 작성하기에 앞서 new.html의 URL을 연결해줍시다. urls.py 내에 원래 있던 코드 밑에 다음과 같이 작성해주세요.

{% highlight html %}
import board.views #board APP의 views 파일을 가져옵니다.
urlpatterns = [
	path('admin/', admin.site.urls),
	path('', board.views.home, name="home"),
	path('new/', board.views.new, name="new"),
	path('create/', board.views.create, name="create"),
]
{% endhighlight %}

* 보시다시피 new에 대한 request와 create에 대한 request를 분리하여 URL로 관리하고 있습니다.

* 이제 views.py로 새로 만들 객체에 대한 정보를 보내줄 수 있도록 new.html을 작성해줍시다. new.html 또한 base.html을 상속받고 있어야함을 잊지 말아주세요. new.html을 열어 다음과 같이 작성해주세요.

{% highlight html %}
{% raw %}
{% extends 'base.html' %}
{% block body %}
<div class="container">
	<form action="{% url 'create' %}" method="POST">
		{% csrf_token %}
		<h2>제목: </h1>
		<input type="text" name="title">  <br>
		<br>
		<h2>내용: </h2>
		<textarea cols=30 rows=10 name="body"></textarea><br>
		<br>
		<input class="btn btn-light" type="submit" value="제출">
	</form>
</div>
{% endblock %}
{% endraw %}
{% endhighlight %}

* form 태그로 우리가 모델로 작성했던 title, body에 대해 입력받고 있습니다. 여기서 pub_date 속성은 입력으로 전해주는 게 아니라 게시글이 작성되는 시점을 자동적으로 처리해야 하므로 템플릿이 아닌 views.py에서 처리해주도록 하겠습니다.

**csrf_token은 POST  방식으로 form 태그가 데이터를 전송할 때 보안을 위해 넣어주는 구문입니다. 이것은 따로 생성해서 넣어주어도 되지만 장고에선 기본적인 템플릿 태그로 지원하고 있기 때문에 우리는 그냥 간편하게 사용해주면 됩니다.**

**또한 method가 POST인 것과 action이 url 'create'인 것에 주목해주세요. 이것은 URL 상에 입력된 데이터를 노출하지 않는 방식(POST)으로 url 'create'로 생성되는 지점에 데이터를 보내겠다는 의미입니다.**


* 이제 views.py에 가서 new화 create에 대한 함수를 작성하여 봅시다.

{% highlight html %}

from django.shortcuts import render, redirect #새로 redirect 함수를 import
from django.utils import timezone #장고 기본 지원, 시간에 대해 다루는 모듈
from .models import Posting

# Create your views here.
def home(request):
	postings = Posting.objects
	content = {'postings' : postings}
	return render(request, 'home.html', content)
	
def new(request):
	return render(request, 'new.html')
	
def create(request):
	post = Posting() #새 객체를 생성
	post.title = request.POST['title']
	post.pub_date = timezone.datetime.now()
	post.body = request.POST['body']
	post.save()
	return redirect('home')

{% endhighlight %}

* 계속해서 말씀드렸던 것처럼 new 함수와 create 함수를 분리해서 작성한 모습입니다. 즉 new.html 자체를 렌더 해주는 함수와 new.html에서 "데이터 새로 넣어주세요" 하고 요청하는 request를 다루는 함수를 따로 작성해서 관리하는 겁니다.

* 여기서 유의미하게 보셔야 하는 점이 몇가지 있는데, 그 중 하나가 새로운 모듈을 import 해주고 있는 ```from django.utils import timezone ``` 입니다. 이 모듈은 시간에 대한 여러 클래스나 함수들을 제공하는 모듈인데 장고 자체 지원이고 그렇게 무겁지 않으니 여러 상황에서 활용하실 수 있습니다. 자세한 사항은 timezone의 정규 Documentation을 확인해주세요.

 * create 함수에서는 새로운 Posting 객체를 일단 빈 객체로 만든 뒤, "데이터 새로 넣어주세요" 하고 날라온 request에 .POST로  빌붙어 함께 날라온 데이터들을 이 빈 객체에 채워주고 있는 모습입니다. 

* 보시다시피 파일 형태가 아닌 데이터 형태들은 모두  **템플릿에서 DB로 데이터가 갈 때는 보안 문제상 무조건 POST 방식을 써야하므로** request.POST["템플릿에서 보낸 데이터 이름"]으로 장고에서 수신이 가능합니다. 실제로 create 함수의 .POST["이름"]으로 받아오는 부분에서 이 "이름"은, new.html 상에 태그 이름으로 붙여준 데이터 이름들을 적어서 받아오는 것입니다. **new.html에서 태그 이름을 바꿨다면 create 함수의 .POST 부분에서도 이름을 바꿔주어야 합니다.**

* 다음으로 유의미하게 보아야 할 것이 redirect 함수 부분입니다. 
  "왜 이 부분에서 그냥 render 함수를 쓰지 않고 redirect 함수를 쓰고 있나요?" 
  왜 그럴까요? 템플릿 파일을 작성하시다 보면 {% raw %} {% url 'home' %} {% endraw %} 등과 같이 urls.py에서 지정한 이름 만으로 손쉽게 URL을 추적할 수 있던 걸 보셨을 겁니다. 이러한 기능을 **reverse** 기능이라고 하는데, 장고에서는 템플릿 뿐만 아니라 views.py 등의 BackEnd 부분에서도 이러한 URL 추적 기능을 손쉽게 사용할 수 있도록 redirect 함수를 제공하고 있습니다. 다시 말하자면 그냥 render 함수를 쓰셔도 좋지만 content(세번째 인자로 key-value 형태로 템플릿에 보내는 것) 인자가 필요 없다면 redirect 함수를 쓰시는 게 더 편할 겁니다. URL 이름만 알면 되니까요!

**_이제 실제로 로컬 서버를 작동시켜 Navbar에 있는 "글쓰기"를 눌러 글을 작성해보세요. home.html 화면에서 저장된 데이터가 잘 뿌려지고 있다면 성공하신 겁니다._**

* 이제 글쓰는 Create를 만들었으니 Read 부분을 만들어 봅시다. Create와 같은 패턴으로 진행되므로 호흡을 좀 빠르게 해보겠습니다. 먼저 urls.py부터 추가해줍시다.

{% highlight html %}
import board.views #board APP의 views 파일을 가져옵니다.
urlpatterns = [
	path('admin/', admin.site.urls),
	path('', board.views.home, name="home"),
	path('new/', board.views.new, name="new"),
	path('post/<int:post_id>/', board.views.detail, name="detail"), #READ부분
	path('create/', board.views.create, name="create"),
]
{% endhighlight %}

* 일반적인 URL path와 조금 다른 점은 바로 눈에 확 들어오는 int:post_id 부분일 것입니다. 이 기능 자체는 path converter 라는 이름을 갖는데, 꺽쇄 안에 콜론을 기준으로 왼쪽은 데이터의 형태, 오른쪽은 데이터의 이름을 의미합니다. 즉 "이 URL 자체는 랜덤하게 들어오는 데이터에 따라 URL의 형태가 바뀔 건데, 여기서 들어오는 데이터 부분이 post_id라는 이름을 갖고 있고, 이건 int(정수) 형태가 될거야. 만약 아니면 무시해." 하고 알려주는 부분이라고 생각하시면 될 것 같습니다.

**장고에서는 템플릿 태그에서 url 'URL 이름' 으로 URL을 컨버팅 할 때, 인자로 변수를 넘겨줄 수도 있습니다. 이 때 넘겨준 인자는 path converter가 자동적으로 해당 형태에 맞는지 체크해 만약 알맞다면 URL 안에 삽입해줍니다.**

* URL을 지정해 주었으니 이제 views.py 에서 반환할 함수를 작성해 봅시다. views.py에서 다음과 같이 작성해주세요.

{% highlight html %}
from django.shortcuts import render, redirect, get_object_or_404
from django.utils import timezone
from .models import Posting

# Create your views here.
def home(request):
	postings = Posting.objects
	content = {'postings' : postings}
	return render(request, 'home.html', content)
	
def new(request):
	return render(request, 'new.html')
	
def create(request):
	post = Posting() #새 객체를 생성
	post.title = request.POST['title']
	post.pub_date = timezone.datetime.now()
	post.body = request.POST['body']
	post.save()
	return redirect('home')
	
def detail(request, post_id):
	post = get_object_or_404(Posting, pk=post_id)
	content = {'post' : post}
	return render(request, 'detail.html', content)
{% endhighlight %}

* 여기서 중요한 함수는 get_object_or_404 함수입니다. 이 함수는 인자를 총 세개까지 받을 수 있는데요, 지금 단계에서 보다 자세히 파고드는 것은 너무 어려우니 일단 간단히 설명하겠습니다.

* 첫번째 인자로는 Model의 이름을 넣어줍니다. 상단에서 import 해준 Posting을 넣어주면 되겠습니다. 두번째 인자로는 보통 pk 값을 넣어줍니다. pk란 primary key의 약자로써 여러 객체들 중에 특정 객체를 찾아야 할 때 사용합니다. 예를 들어 현대 자동차 100대가 있고 이 중에서 특정 차량 한대를 찾아야 한다면 차량번호를 보는 것과 같이, 차량번호의 역할을 pk가 해준다고 생각하시면 될 것 같습니다. 이 pk의 값이 될 post_id는 detail 함수 자체가 request와 함께 인자로 받고 있습니다.

* 이 두 인자를 바탕으로 get_object_or_404 함수는 객체를 찾으면 해당 객체를 반환하고, 못찾으면 404 에러를 반환합니다.

* detail 함수는 이렇게 찾은 객체를 detail.html에 render 함수를 통해 보내주는 역할을 합니다. 이제 구체적으로 detail.html에서 받은 객체를 어떻게 띄워줄 것인지 작성해봅시다. detail.html에서 다음과 같이 작성해주세요.

{% highlight html %}
{% raw %}
{% extends 'base.html' %}
{% block body %}
<h1>Detail of <span>{{post.title}}</span></h1>
<p>{{post.pub_date}}</p>
<p>{{post.body}}</p>
{% endblock %}
{% endraw %}
{% endhighlight %}

* 찾아온 객체를 key-value 형태에서 'post'라는 문자열로 보내주었기 때문에 장고 변수 템플릿(중괄호 두개 형태)으로 사용이 가능한 모습입니다.

* detail.html이 잘 작동하고 있는지 확인하기 위해 home.html에서 detail.html로 들어가는 통로를 하나 만들어 줍시다. h1 태그로 감싸져 있는 posting.title 부분을 다음과 같이 수정해주세요.

{% highlight html %}
{% raw %}
<a  href="{% url 'detail' posting.id %}"><h1>{{posting.title}}</h1></a>
{% endraw %}
{% endhighlight %}

**_이제 다시 로컬 서버를 켜서 home.html에 있는 타이틀 부분을 눌러 detail.html로 접속해봅시다. 잘 접속된다면, 축하드립니다! 이제 CR부분을 완성하신 겁니다!_**


## STATIC / MEDIA

* 이제 본격적으로 장고에서 어떻게 이미지, 동영상, CSS, JS 등을 관리하고 화면에 띄워주는 지에 대해서 알아봅시다. 이러한 파일들을 장고에서는 두가지 종류로 구분하여 관리합니다. 첫번째는 서버 관리자가 미리 서버에 올려두는 파일, 두번째는 사이트 이용자가 올리는 파일입니다. 첫번째를 Static이라고 하고, 두번째를 Media라고 합니다.

* 쉽게 예를 들어 볼까요? 네이버에 들어가 봅시다. 네이버에 들어갔을 때 보이는 네이버 로고 이미지는 Static입니다. 그리고 우리가 블로그에 글을 작성할 때 넣는 이미지는 Media입니다.

![naver_logo](https://user-images.githubusercontent.com/46686577/55428080-58e69180-55c3-11e9-8dad-a8d9f0bf4e1c.png)

* 더불어 Static 파일과 Media 파일의 가장 큰 차이점은 모델에 귀속되느냐 그렇지 않느냐가 있겠네요. Static은 서버에 바로 귀속되지만 Media 파일은 모델을 통해 DB에 귀속됩니다.

* 우리는 먼저 Static에 대해서, 어떻게 서버 관리자, 즉 웹페이지 개발자가 미리 이미지 파일을 올려놓고 이를 관리하는 지에 대해 배워볼 것입니다. 이번 파트는 CR 복습 때 사용했던 프로젝트는 그대로 활용하여 진행해보도록 하겠습니다.

## STATIC

* Static 파일은 앞서 설명했듯이 미리 서버에 올려놓는 파일입니다. 따라서 우리는 어떤 파일이 어디에 들어갈지 사전에 설정해둘 수 있습니다. 즉 Settings.py에서 Static 파일들이 들어갈 경로를 지정해줄 수 있는데요, 진행하기에 앞서 에러를 방지하기 위해 먼저 다음 명령어를 터미널에 쳐줍시다.

{% highlight html %}
	python manage.py collectstatic
{% endhighlight %}
* 가끔씩 Static Root 경로를 지정해주면서 Admin 페이지에 CSS가 로드되지 않는 현상이 일어나기 때문에 방지 차원에서 해주는 겁니다.

* 이제 본격적으로 Settings.py에서 Static 경로들을 설정해줍시다. Settings.py 맨 아래 쪽에 보시면

{% highlight html %}
	STATIC_URL =  '/static/'
{% endhighlight %}

* 이라고 되어있는 부분이 있을 겁니다. 이 밑에 다음과 같이 붙여주세요. (막간은 이해를 위해 덧붙였습니다.)

**막간 - "근데 이 STATIC_URL은 뭔가요?"**

* URL 템플릿 태그에서 url 'URL 이름' 과 같이 사용해보셨을 겁니다. 마찬가지로 static 또한 불러오는 템플릿 태그가 있습니다. 이 태그는 URL 태그와 비슷한 방식으로 동작합니다. 실제로 웹페이지에서 서버에 있는 파일을 불러올 때 경로로써 접근하기 때문에 우리가 urls.py에서 URL path를 지정해주듯이 Static 파일 또한 기본 경로를 지정해줘야 합니다. STATIC_URL은 밑에서 나올 {% raw %} {% static '파일 경로와 이름' %} {% endraw %} 템플릿 태그를 사용할 때 맨 앞에 붙여줄 기본 경로를 의미한다고 이해하시면 됩니다.

{% highlight html %}
STATICFILES_DIRS = [
	os.path.join(BASE_DIR, 'board', 'static'),
]
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
{% endhighlight %}

* STATICFILES_DIRS는 BASE_DIR(이 경우 프로젝트 최상위 폴더입니다)에서부터 어떠한 폴더에 Static 파일들이 저장되어 있는지를 나타내는 부분입니다.

* STATIC_ROOT는 Collectstatic 작업을 통해 Static 파일들이 모여질 목적지 경로를 지정하는 부분입니다.

* 예를 들어 설명하자면, 우리는 App 폴더 밑에 static이라는 폴더를 만들어 그 밑에 img 폴더를 만들고 그 안에 이미지 파일을 넣을 것입니다. 따라서 STATICFILES_DIRS에는 board App 안에 static이라는 폴더를 지정해줍니다, 이로써 img 폴더가 위치한 레벨 전체가 Static 파일의 경로로 잡히게 됩니다.

* 그리고 collectstatic을 하게 되면, BASE_DIR(최상위 폴더) 바로 밑에 static이라는 폴더가 존재한다면 그 폴더에, 존재하지 않는다면 새로 그 이름과 같은 폴더를 만들게 됩니다. 그 후 해당 폴더로 STATICFIELS_DIRS 경로에 지정되어 있는 모든 static 파일들을 모으게 됩니다.

**"App에 static 파일이 이미 있는데 collectstatic으로 최상위폴더(STATIC_ROOT)에 굳이 수고를 들여가며 새로 복사해주는 이유가 뭔가요?"**

* 그 이유는 바로 우리가 개발중이기 때문입니다. 개발 단계에서는 App별로 static 폴더를 분리하여 관리하는 것이 훨씬 편합니다, 우리는 각 App 마다의 이미지 파일, 동영상 파일, CSS, JS 등을 관리하기만 하면 되니까요. Django는 contrib라는 친구가 Finder라는 도구를 사용해 파일 트래킹을 해주기 때문에 개발 단계(Debug=True)에서는 App 폴더에 있는 static 파일에 대한 URL을 자동으로 계산해줍니다. 따라서 우리가 개발하면서 App폴더에 static 폴더를 만들어 그 안에 파일을 넣고 템플릿에서 static 태그를 사용해도 Django는 알아서 해당 파일에 대한 URL을 산출하여 파일을 띄워줍니다. 하지만 배포 단계에서는 이렇게 하면 안됩니다. 조금 복잡한 이야기가 이면에 존재하므로 자세한 설명은 생략하고, 간단히 말하면 속도/성능에 대한 문제와 Django Server App과 실제 Server가 다르기 때문이라고만 알아둡시다. 따라서 배포 단계를 위해선 collectstatic을 해서 STATIC_ROOT 경로에 static 파일들을 모아주는 것이 Django Style입니다. 

**"경로와 루트를 지정해주고 collectstatic을 하려니까 이미 있는 파일들 한번더 collect하겠냐는 메세지가 뜨는데요?"**
* 아까 추가한 코드에 다음과 같은 코드를 하나 더 추가해주세요.

{% highlight html %}
STATICFILES_FINDERS = (
	'django.contrib.staticfiles.finders.FileSystemFinder',
)
{% endhighlight %}

* 파일에 대한 Path를 찾아주는 Finder라는 녀석입니다.

### 이제 실제로 Static 이미지를 넣어서 home.html에 띄워볼까요?

* 저는 제가 10년 동안 애청 중인 게임 스트리머인 '우왁굳'의 사진을 넣어볼 예정입니다. 넣을 사진은 임의대로 구해주세요. 제가 구한 사진은 다음과 같이 생겼습니다.

![warkdoo](https://user-images.githubusercontent.com/46686577/55428089-5ab05500-55c3-11e9-9998-68bca12f47b5.png)

*  이제 App 안에 static이라는 폴더를 만들고 그 안에 img라는 폴더를 만들어 그 폴더 안에다가 우리의 이미지를 넣어줍시다.

**"왜 바로 static에 넣지 않고 굳이 img 폴더를 만드나요?"**

* 지금은 이미지 파일만 넣지만 우리는 static으로 다룰 것들이 많습니다, 대표적으로는 CSS와 JS가 있겠죠. 이런 파일들은 카테고리가 명확하기 때문에 따로 폴더를 두어 관리하는 것이 유지보수에 유리합니다. 따라서 미리 연습한다고 생각해주세요.

* 다음과 같이 폴더 계층을 만들어주시면 됩니다.

![static_folder](https://user-images.githubusercontent.com/46686577/55428085-5a17be80-55c3-11e9-8994-7703854fc64c.png)

* css 폴더는 이해를 돕기 위해 제가 임의로 추가한 것이니 굳이 신경쓰지 않으셔도 좋습니다.

* 이제 home.html에서 이 static 파일, 즉 이미지를 띄워줍시다. 장고에선 템플릿 파일에서 static 파일을 불러오기 위해 써주어야 하는 템플릿 태그가 있습니다. 우리가 앞서 사용해보았던 extends 와 같이 서버 내에 존재하는 파일을 템플릿으로 불러올 때 필요한 절차라고 이해해주세요.

{% highlight html %}
{% raw %}
	{% load static %}
{% endraw %}
{% endhighlight %}

* extends 템플릿 태그가 템플릿 파일을 불러오는 것이라면 load static 태그는 static 파일을 불러오는 태그입니다. extends 태그 밑에, block 태그 위에 이 태그를 넣어주세요.

* 이제 실제로 이미지 파일을 home.html 안에 넣어줍시다. 다음과 같은 코드를 이미지가 뜨길 원하는 임의의 위치에 넣어주세요. 저는 다음과 같이 작성하였습니다.

{% highlight html %}
{% raw %}
{% extends 'base.html' %}
{% load static %}
{% block body %}
<div  class="container">
	<!-- 저는 이 위치에 넣었습니다 -->
	<img  src="{% static 'img/wakdoo.png' %}">
	<!-- 저는 이 위치에 넣었습니다 -->
	<h1>Posting List</h1>
	{% for posting in postings.all %}
		<div  class="container">
			<h1>{{posting.title}}</h1>
			<p>{{posting.pub_date}}</p>
			<p>{{posting.body}}</p>
		</div>
	{% endfor %}
</div>
{% endblock %}
{% endraw %}
{% endhighlight %}

* 이제 로컬 서버를 실행하여 home.html을 확인해주세요. 다음과 같이 나오면 성공한 것입니다.

![static_check](https://user-images.githubusercontent.com/46686577/55428084-597f2800-55c3-11e9-8c49-a18386889acc.png)

**저는 안뜨는데요?!**
* 페이지 캐시가 남아있어 브라우저가 웹 페이지를 새로 로드하지 않을 수도 있습니다. 쿠키를 삭제하고 다시 로드해보세요. 그래도 안된다면 Finder가 길을 헤메고 있을 수도 있으니 collectstatic을 통해 STATIC_ROOT에 파일을 모아준 뒤 다시 로드해 봅시다.

## MEDIA

* Static 파일과 달리 Media 파일은 어떤 파일이 들어올지 서버 관리자 입장에서 알 수 없습니다. 사이트 유저가 임의대로 자신이 원하는 폴더를 올리기 때문입니다. 따라서 Media 파일은 Static 파일과 다른 경로와 방식으로 관리해주어야 합니다. 이 말은 곧 settings.py에서 Media와 관련된 경로도 따로 지정해야 한다는 뜻이죠.

* settings.py 에서 우리가 static 경로를 지정해주었던 밑부분에 다음과 같이 작성해주세요.

{% highlight html %}
#Media 루트 설정
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
MEDIA_URL =  '/media/'
{% endhighlight %}

* STATIC_URL과 같이 Media 파일 또한 기본 URL과 파일이 저장될 장소를 지정해주어야 합니다. 위 코드는 STATIC_URL과 STATIC_ROOT와 같은 원리라고 이해해주세요.

**"STATICFILES_DIRS는 있는데 왜 MEDIAFILES_DIRS 설정은 없죠?"**

* 앞서 설명했듯이 실제 웹페이지가 불러오는 Static 파일들은 App 밑에 있는 static 폴더가 아니라 collectstatic으로 복사해둔 최상위 폴더의 static 폴더에 있는 파일들입니다. STATICFIELS_DIRS는 이 collectstatic을 위한 경로 지정일 뿐인 것이지요. Media 파일은 서버 관리자 입장에서 올려두는 파일이 아니라 웹사이트 이용자가 올리는 파일이기 때문에 App 차원에서 직접 관리할 필요가 없습니다. 바로 최상위폴더에 저장하여 DB와 연결한 뒤 '어디에 저장해놨다' 라는 표시만 해두면 서버 관리자는 더 신경쓸 필요가 없는 것입니다. 따라서 MEDIAFILES_DIRS 설정은 없습니다.

* 이제 Static과는 다른 Media만의 조금 독특한 설정을 해주어야 합니다. 이 부분은 직접 코드를 보면서 설명하겠습니다. 일단 urls.py로 가서 다음과 같이 코드 전문을 작성해주세요.

{% highlight html %}
from django.contrib import admin
from django.urls import path
from django.conf import settings #settings.py를 갖고옵니다
from django.conf.urls.static import static #static 함수를 갖고옵니다
import board.views

urlpatterns = [
	path('admin/', admin.site.urls),
	path('', board.views.home, name="home"),
	path('new/', board.views.new, name="new"),
	path('post/<int:post_id>/', board.views.detail, name="detail"), #READ부분
	path('create/', board.views.create, name="create"),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

#urlpatterns += static(settings.MEDIA_URL,document_root=settings.MEDIA_ROOT)
#2가지 방식으로 사용 가능
{% endhighlight %}

* 먼저 새로 import 한 것부터 살펴 볼까요? 일단 우리가 앞서 작성했던 여러 설정 코드들이 있는 settings.py를 갖고 왔습니다. 그리고 static 함수라는 것을 갖고 왔네요. (이러한 import 관련 경로들은 굳이 외우시지 않아도 좋습니다. 필요할 때마다 구글링하거나 이전 자료들을 찾으면서 사용해주세요)

* 먼저 static 함수에 대해 설명하겠습니다. 일반적으로 우리가 urls.py에서 작성하는 URL 경로들은 '웹페이지'의 경로들입니다. "home.html을 띄워주세요. 아, 템플릿 태그에서는 {% raw %} {% url 'home' %} {% endraw %} 으로 사용할 거니까 그렇게 해주세요." 와 같은 필요에 따라 작성하는 것입니다. 

* 그런데 실제 웹사이트에서는 '페이지'에 대한 경로 뿐아니라 '파일'에 대한 경로도 필요할 때가 있습니다. 앞서 Static 파일 또한 "STATIC_URL로 설정한 폴더 밑에 있는 img 폴더 밑에 있는 이미지 파일을 불러와줘" 와 같은 절차로 파일이 불러온 겁니다. 다만 Static 파일의 경우 우리는 어떤 경로에 어떤 파일 이름이 있는 지를 미리 알고 있지만 Media 파일은 유저가 임의대로 업로드하기 때문에 서버 관리자는 그 파일이 있는지 없는지, 파일이 있다면 그 이름은 무엇인지 알지 못합니다.

* static 함수는 이런 Media 파일 특성상 따로 접근 URL을 관리해주기 위해서 존재하는 함수입니다. "아 만약에 Media 파일 있으면 URL 맨 앞에 settings.py에서 MEDIA_URL로 지정해둔 경로 붙이고 실제 파일은 MEDIA_ROOT에서 갖고와줘." 이런 뜻으로 이해해주세요.

* 여담으로 보기가 불편하시다면 주석처리한 방법으로도 사용이 가능합니다 :)

### 이제 실제로 Media 파일을 올려봅시다.

* 일단 우리는 웹사이트 이용자가 이미지 파일을 업로드할 수 있도록 해볼 겁니다. models.py에 가서 다음과 같이 이미지에 대한 속성을 하나 더 우리의 Posting 모델에 붙여줍시다. 즉 게시글 자체에 이미지 저장 장소가 하나 더 추가되는 겁니다.

{% highlight html %}
from django.db import models

class Posting(models.Model):
	title = models.CharField(max_length=100)
	pub_date = models.DateTimeField("date_published")
	image = models.ImageField(upload_to='images/')
	body = models.TextField()

	def __str__(self):
		return  self.title
{% endhighlight %}

* 여기서 인자로 들어가는 upload_to는 MEDIA_ROOT 내에 어떤 폴더에 저장될 지를 지정해주는 부분입니다. 우리는 현재 이미지를 다루고 있지만 비디오나 일반 파일 등도 웹사이트 이용자가 업로드할 수 있기 때문에 개별 관리 하기 위함이라고 이해 해주세요.

* 모델을 수정하였으니 터미널에서 ``` python manage.py migrate``` 해주세요.

**"뭘 설치하라는데요?"**
* 현재 장고에서 지원하는 ImageField는 규격화된 이미지 처리를 위해 Pillow를 사용하고 있습니다. 설치하라니까 설치 해야지요. ```pip install Pillow```를 터미널에 입력해서 설치해줍시다.

![pillow_error](https://user-images.githubusercontent.com/46686577/55428082-597f2800-55c3-11e9-9e40-02da6a94c535.png)

**"자꾸 모델/DB 관련해서 오류가 떠요!"**

* 걱정 마세요. 아주 흔한 일이고 당연한 겁니다. DB는 장고와 별개이기 때문에 우리가 직접 접근할 수 없으므로 이런 일이 자주 일어납니다. 여기서 꿀팁을 하나 드릴게요.

**_모델, DB 관련해서 오류가 뜬다면 migrations 폴더 안에 init.py 파일을 빼고 모두 삭제해주세요. 그리고 최상위 폴더의 db.sqlite3 파일도 삭제해주세요. 그 다음 터미널에서 다시 ```python manage.py makemigrations``` ```python manage.py migrate``` 해주세요. 이게 훨씬 마음 편할 상황이 많으실 겁니다._**

* 이제 admin 사이트에 admin 계정으로 접속하신 뒤 Posting 테이블에서 이미지 파일이 업로드 가능하신 걸 확인하실 수 있으실 겁니다.

![admin_image_upload](https://user-images.githubusercontent.com/46686577/55428073-57b56480-55c3-11e9-8af7-b59df37a5231.png)

* 이렇게 업로드한 Media 파일을 home.html에서 한번 불러와 볼까요? home.html에서 다음과 같이 추가해주세요.

{% highlight html %}
{% raw %}
{% extends 'base.html' %}
{% load static %}
{% block body %}
<div  class="container">
	<img  src="{% static 'img/wakdoo.png' %}">
	<h1>Posting List</h1>
	{% for posting in postings.all %}
		<div  class="container">
			<a  href="{% url 'detail' posting.id %}"><h1>{{posting.title}}</h1></a>
			<!-- 이 부분이 추가됩니다 -->
			<img  src="{{ posting.image.url }}">
			<!-- 이 부분이 추가됩니다 -->
			<p>{{posting.pub_date}}</p>
			<p>{{posting.body}}</p>
		</div>
	{% endfor %}
</div>
{% endblock %}
{% endraw %}
{% endhighlight %}

*  {% raw %} {% load static %} {% static '이름' %} {% endraw %} 콤보로 자동으로 URL이 연결되는 Static 파일과는 다르게, 우리가 urls.py에서 static 함수로 설정한 경로를 바탕으로 "객체 이름.객체 속성.url" 함수 이용을 통해 Media 파일에 대한 URL을 연결하고 있는 모습을 확인하실 수 있습니다.

*  마찬가지로 home.html을 수정했으니 detail.html 부분도 수정해주도록 하겠습니다. 간단하게 title 밑부분에 다음과 같이 추가해 주세요.

{% highlight html %}
{% raw %}
	<img  src="{{ post.image.url }}">
{% endraw %}
{% endhighlight %}

* 이제 admin 페이지에서 데이터를 몇개 추가하신 뒤 home.html 과 detail.html에서 실제로 Media 파일이 뜨고 있는지 확인해주세요. 잘 뜬다면 성공하신 겁니다!

### 심화 - create 함수를 통해 웹페이지 이용자가 실제로 업로드한 Media 파일을 DB에 저장하기

* 이 부분은 아마 온라인 강의에서는 다루지 않는 부분일 겁니다. 그저 admin 단계에서 Media 파일을 추가하고 삭제하는 것은 "웹사이트 이용자가 업로드하는 것"이 전제인 Media 파일과는 조금 어긋난다고 생각하였기 때문에 제가 따로 추가하는 부분이니 참고해주세요. 더불어 그렇게 어렵지 않으니 걱정 않으셔도 좋습니다 :)

* 우리는 이미 모델에 이미지 속성을 추가하였으니 new.html에서 실제로 웹사이트 이용자가 이미지를 업로드 할 수 있도록 수정해줍시다! 몇가지 설명할 부분이 있으니 코드와 함께 설명하겠습니다.

{% highlight html %}
{% raw %}
{% extends 'base.html' %}
{% block body %}
<div class="container">
	<!-- 추가된 부분입니다 -->
	<form action="{% url 'create' %}" method="POST" enctype="multipart/form-data">
	<!-- 추가된 부분입니다 -->
		{% csrf_token %}
		<h2>제목: </h1>
		<input type="text" name="title">  <br>
		<br>
		<!-- 추가된 부분입니다 -->
		<h2>이미지: </h2>
		<input  type="file"  name="image">  <br>
		<!-- 추가된 부분입니다 -->
		<h2>내용: </h2>
		<textarea cols=30 rows=10 name="body"></textarea><br>
		<br>
		<input class="btn btn-light" type="submit" value="제출">
	</form>
</div>
{% endblock %}
{% endraw %}
{% endhighlight %}

* 먼저 form 태그에 추가된 ```enctype="multipart/form-data"```  부분은 인코딩에 대해 정의하는 부분이라고 이해해주세요. 우리가 현재 웹사이트 이용자에게 업로드 하도록 새로 촉구한 부분은 '이미지'이지만 실제로 form 태그 자체는 '이미지'와 '일반 파일'을 동격의 파일로 취급합니다. 따라서 이렇게 POST 방식으로 보내지는 파일에 대한 보안을 위해 표준 인코딩 타입을 정해주어야 "아, 이것은 규격에 맞는 파일이군" 하고 서버가 받을 수 있게 됩니다.

* 앞서 설명했듯이 따라서 새로 추가된 input의 type은 file이 됩니다. 모델과의 동기성을 맞추기 위해 이 input의 이름도 모델 속성의 이름을 따라 image로 지어줍시다.

* 이제 이렇게 new.html에서 작성된 데이터가 create 함수로 보내졌으니 이를 처리할 수 있도록 views.py에서 create 함수를 수정해줍시다. 다음과 같이 수정해주세요.

{% highlight html %}
def create(request):
	post = Posting()
	post.title = request.POST['title']
	post.pub_date = timezone.datetime.now()
	post.image = request.FILES['image'] #이 부분이 추가되었습니다
	post.body = request.POST['body']
	post.save()
	return redirect('home')
{% endhighlight %}

* 일반 데이터의 경우 POST 형식으로 보내진 데이터는 모두 "저기요, 서버님! 데이터 새로 넣어주세요~"하는 request가 들고 있는 POST라는 바구니에 담겨 있습니다. 하지만 파일의 경우 특별히 인코딩되어 날라오므로 POST와 다르게 FILES라는 바구니에 담겨 있다고 이해 해주세요. 따라서 객체의 image 필드에 form에서 날라온 'image' 라는 input에 담겨 있는 파일을 저장해주고 있는 것입니다.

* 이제 로컬 서버를 실행하여 Navbar에 있는 '글쓰기' 버튼을 눌러 우리의 이미지가 잘 저장되는지 학번 확인해봅시다. 잘 뜬다면 성공하신 겁니다!


## Thumbnail

* 카카오톡을 보시면, 내가 등록한 커다란 프로필 이미지가 대화창에서는 동그랗고 조그맣게 리사이징 되는 것을 보신 적이 있으실 겁니다. 우리는 동그란 이미지를 넣지 않았음에도 카카오톡이 알아서 동그랗게 잘라주는 것이죠. 이런 작업 처리를 Thumbnail이라고 합니다. 장고 또한 이런 기능을 사용할 수 있습니다. 정확히는 지원하는 외부 모듈이 여럿 존재합니다. 이번에는 그 중 imagekit을 사용해 볼 겁니다.

* 백문이 불여일타라고 바로 한번 해봅시다. 일단 터미널에 다음과 같이 입력해서 모듈을 설치해주세요.

{% highlight html %}
	pip install pillow django-imagekit
{% endhighlight %}

* 만약 Pillow가 이미 설치되어 있다면 Pillow에 대한 설치는 무시될 겁니다. 그래도 혹시 모르니 같이 넣어주세요.

* 설치가 완료 됐다면 새로운 모듈이 설치되었으니 사용하고 싶다고 settings.py에 알려주어야 합니다. INSTALLED_APPS에 다음과 같이 추가해주세요.
{% highlight html %}
INSTALLED_APPS = [
	'django.contrib.admin',
	'django.contrib.auth',
	'django.contrib.contenttypes',
	'django.contrib.sessions',
	'django.contrib.messages',
	'django.contrib.staticfiles',
	'board.apps.BoardConfig',
	'imagekit', #imagekit을 추가해주세요
]
{% endhighlight %}

* 이제 models.py로 가서 imagekit으로 리사이징한 이미지를 속성으로 새로 추가해주어야 합니다. 일단 코드를 다음과 같이 수정해주세요.

{% highlight html %}
from django.db import models
from imagekit.models import ImageSpecField #썸네일 제작의 중추합수입니다
from imagekit.processors import ResizeToFill #썸네일 리사이징 함수입니다

class Posting(models.Model):
	title = models.CharField(max_length=100)
	pub_date = models.DateTimeField("date_published")
	image = models.ImageField(upload_to='images/')
	image_thumnail = ImageSpecField(source='image', processors=[ResizeToFill(120,100)], format="JPEG", options={'quality':60})
	body = models.TextField()

	def __str__(self):
		return  self.title
{% endhighlight %}

* 먼저 새로 import 한 두 함수를 보겠습니다. ImageSpecField는 imagekit 모듈을 설치하면서 새로 models에 추가된 함수입니다. 간단히 말해서 썸네일 이미지 파일 제작의 코어 함수라고 생각하시면 될 것 같습니다. ResizeToFill은 이름 그대로 썸네일로 만들 이미지에 대한 리사이징 기능을 제공하는 함수입니다. 인자가 두 개인데, 차례대로 가로-세로입니다. 단위는 픽셀 단위이니 참고해주세요.

* 객체 속성으로 추가하는 부분을 볼까요? ImageSpecField 함수로 만든 썸네일 이미지를 image_thumnail이라는 객체 속성 변수에 담겠다는 의미입니다. 

* ImageSpecField 함수에 인자로 들어가는 부분들을 봅시다. source는 어떤 이미지 파일을 썸네일로 만들 것인지를 지정해주는 부분입니다. 우리는 모델에 귀속되는 Media 파일의 이름을 image라고 지정했으므로 source를 image로 정해주었습니다. 

* processors는 말 그대로 어떤 처리를 할 것인지를 정해주는 부분입니다. 우리는 리사이징 할 것이므로 ResizeToFill 함수를 이용합니다. 

* format 부분은 이 함수를 통해 제작된 이미지 파일이 어떤 확장자로 저장될 것인지를 정해주는 부분입니다. 제가 올리는 Media 이미지 파일은 모두 PNG 형태지만 이 함수를 통해 JPEG 형태로 전환되는 것을 확인하실 수 있습니다. 

* options 부분은 기타 속성들을 정해주는 부분입니다. 예를 들어 quality 즉 화질을 정할 수 있는데, 화질에 따라 크기가 달라지므로 우리는 일반적인 값 60 정도를 줘봅시다.

* 이제 모델을 수정하였으므로 migrate 해주면 됩니다.

{% highlight html %}
	python manage.py migrate
{% endhighlight %}

### 이제 home.html에서 보여지는 사진을 Thumbnail로 바꿔봅시다.

* 간단합니다. home.html에서 객체에 붙어있는 이미지를 보여주는 부분을 썸네일로 바꿔주기만 하면 됩니다. 다음과 같이 수정해주세요.

{% highlight html %}
{% raw %}
	<img  src="{{ posting.image_thumnail.url }}">
{% endraw %}
{% endhighlight %}
* 이제 로컬 서버를 켜서 실제로 썸네일 이미지가 잘 나오고 있는지 확인해줍시다. 다음과 같이 나오면 성공하신 겁니다!
  ![Thumbnail_test](https://user-images.githubusercontent.com/46686577/55428087-5a17be80-55c3-11e9-941e-ac59840a5fa0.png)

<blockquote>
이상 이번 9번째 세션을 마무리하도록 하겠습니다.
따라와 주신 여러분 모두 수고 많으셨습니다. 질문이 있으시다면 주저 말고 연락해주세요.
감사합니다.
</blockquote>  

* PPT 참고 

[9th_session.pdf](https://github.com/TaeHun-Lee/Picture_test/files/3060480/9th_session.pdf)
