---
layout: post
title: 05. HTTP Methods / Wordcounter / Parsing
category: 7th
---
세션 5 - prod. 인건


# HTTP Methods

### HTTP란??

HTTP는 웹 클라이언트와 웹 서버가 의사소통을 할 때에 있어서 사용되는 프로토콜(통신 규약)을 말합니다. 생소한 용어들이 많이 나와서 어려우시죠?

맥도날드에 비유해서 한 번 설명해 보겠습니다. 제가 맥도날드에 들어가서 “상하이 스파이시 치킨 버거 세트”를 달라고 말하면, 주방에서는 그 메뉴를 만들어서 저에게 전달해 줍니다. 제가 클라이언트라고 하면 주방은 저에게 반응하는 것이죠.

웹 서비스도 이와 똑같습니다. 인터넷 브라우저를 통해 네이버에 접속해서 멋쟁이 사자처럼에 대한 검색 결과를 보여줄 것을 요청하면 네이버의 웹 서버가 저의 요청에 반응하여 그에 대한 검색 결과를 보여줍니다. 즉 인터넷 브라우저와 같은 프로그램들이 웹 클라이언트, 클라우드에 올라가 있는 네이버와 같은 서비스들이 서버라고 할 수 있죠.

이렇게 브라우저와 네이버의 서버가 요청과 반응을 주고 받을 때 사용해야하는 특정한 규칙을 HTTP라고 합니다.

### HTTP 요청 방식

앞서 설명드린 것 처럼, 저희가 웹 브라우저를 통해 웹 서버와 요청, 반응하게 되는데 클라이언트가 서버에 요청을 하는 방법은 딱 정해져 있습니다. 총 4가지에 방식이 있는데 그에 대해 하나씩 알아보도록 하겠습니다.

→ *GET 방식*

GET 방식의 경우 웹 클라이언트가 서버에 특정 HTML 파일을 달라고 요청을 하는 경우 입니다. 우리가 웹서핑을 할 때 가장 많이 사용되는 방법 중에 하나일 것입니다. 제가 네이버에 검색어를 입력해서 검색을 하는 경우, 블로그 링크를 클릭해서 특정 사이트에 들어가는 경우가 모두 GET 방식에 해당합니다.

→ *POST 방식*

POST 방식은 웹 클라이언트가 어떠한 것을 처리해달라고 서버에 정보를 보내는 경우를 말합니다. 우리가 특정 사이트에 로그인을 하거나, 게시물을 작성하거나 하는 모든 과정이 POST 방식을 사용합니다.

→ *PUT 방식*

PUT 방식은 서버에 존재하는 정보를 수정할 때 사용 되는 방식입니다.

→ *DELETE 방식*

DELETE 방식은 서버에 존재하는 정보를 삭제할 때 사용 되는 방식입니다.

오늘 세션을 통해서는 가장 먼저 GET 방식에 대한 실습을 해볼 예정입니다.

---

# Wordcounter 만들어보기!

## 00. 프로젝트 구상하기

Wordcounter 프로젝트는 말 그래도 단어를 세어주는 사이트를 만들어보는 프로젝트 입니다. 텍스트 창에 세어보고 싶은 텍스트를 넣고 입력을 하면, 같은 단어가 몇 번 사용 되었는지를 보여주는 것이 이 프로젝트의 최종 목표 입니다. 

---

## 01. 프로젝트 생성하기

가장 먼저 Django 프로젝트를 만들어줘야 합니다.

→ **가상 환경에 접속하는게 항상 먼저 이루어져야 합니다.** 
{% highlight bash %}
#myvenv라는 이름을 가진 가상환경을 현재 디렉토리에 생성하기
$ python3 -m venv myvenv

# 가상환경 실행하기
#맥의 경우
$ source myvenv/bin/activate 
#윈도우의 경우
$ source myvenv/Scripts/acitvate 

#아래와같이 나오기 시작하면 가상환경 접속 성공!
(myvenv)$ 
{% endhighlight%}

**→ django 설치하기 **
{% highlight bash %}
$ pip install django
{% endhighlight%}

**→ 프로젝트 만들기**
{% highlight bash %}
#Firstproject라는 이름을 가진 장고 프로젝트를 생성
$ django-admin startproject Firstproject
{% endhighlight%}

**→ 프로젝트 내에 앱 만들기** 
{% highlight bash %}
#먼저 Firstproject 디렉토리로 이동해야합니다
$ cd Firstproject 

#그 후 앱을 생성합니다.
$ python manage.py startapp wordcount
{% endhighlight%}
---

## 02. Templates 만들어주기

먼저 만들어진 wordcount 앱의 존재를 프로젝트에 알려줘야 앱을 프로젝트 내에서 사용할 수 있게 됩니다. 

Firstproject > settings.py 파일을 켜주시면 됩니다. 조금만 내려보시면 아래와 같이 설치된 앱들에 대한 정보가 나와 있을 것 입니다. 
{% highlight python %}
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
{% endhighlight %}

가장 아래에 아래와 같이 wordcount 앱에 대한 정보를 입력해주면 됩니다. 
{% highlight python %}
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
        'wordcount.apps.WordcountConfig',
]
{% endhighlight %}

자, 여기서 잠깐 Template가 뭐였는지 한번 복습하고 갑시다. Template는 Django가 사용하는 MTV 패턴에서 T에 해당하는 Template를 말합니다. 더 통상적으로 사용되는 MVC 패턴에서의 V를 뜻하는 View, 즉 사용자에게 보여주는 사이트 화면을 말하죠. 자 그럼 이 사실을 잊지 말고 Template를 한번 만들어 봅시다. 

가장 먼저 Template들을 담을 디렉토리를 하나 만들어줘야 합니다. wordcount 안에 "templates"라는 이름의 폴더를 만들어주면 됩니다. 만들어진 templates 폴더 안에 **"home.html", "result.html", "about.html"** 의 이름을 가진 파일 3개를 만들어줍니다. "home" 페이지는 프로젝트에 접속 했을 때 나오는 루트 페이지를, "about" 페이지는 만들어진 Wordcounter 에 대한 설명을, "result" 페이지는 단어를 카운트 한 결과를 보여주는 페이지 입니다. 

이러한 3개의 템플렛은 2가지 속성으로 구분 할 수 있습니다. "home"과 "about"은 항상 같은 페이지를 보여주는 반면 "result" 페이지는 그때 그때 입력된 값에 따라서 다른 결과를 보여주는 페이지 일 것입니다. 이러한 속성을 기억하면서 코딩을 하면 조금 더 쉽게 다가갈 수 있을 것입니다. 

home.html 파일과 about.html 파일을 열어서 해당 페이지에 html 코드를 작성해봅시다. 
{% highlight html %}
<!-- home.html -->
<h1> Wordcount Application </h1> 

<!-- about.html -->
<h1> About Wordcount </h1>
{% endhighlight %}
이런식으로 일단 간단하게 작성해주시면 됩니다. 

---

## 03. View 만들기

보여질 페이지를 만들었으니 이제 페이지를 연결해서 접속 가능하게 만들어야 합니다. 

wordcount >> views.py 파일을 열어주세요. 
{% highlight python %}
from django.shortcuts import render

# Create your views here.
{% endhighlight %}
라는 내용이 보이면 잘 찾으신겁니다. 이 뷰 파일 내에는 특정 url로 누군가 접속했을 때, 그 접속을 어떻게 처리할 것인가에 대한 내용을 적는 곳입니다. 앞서 말한 것 처럼 home.html과 about.html은 그냥 단순하게 그 사이트에 접속하면 되는 것이기 때문에, 아래와 같이 코드를 작성해주시면 됩니다. 
{% highlight python %}
def home(request): 
    return render(request, 'home.html')

def about(request):
    return render(request, 'about.html')
{% endhighlight %}
---

## 04. URLconf 연결하기

이제 view까지 다 만들었으니 이제 해당 view로 갈 url을 연결해줘야 합니다. 쉽게 말해서 기능은 다 만들었으니 그 기능으로 가는 길을 뚫어주는 차례인 것 입니다.

firstproject >> urls.py 파일을 열어주세요.

{% highlight python %}

from django.conf.urls import url
from django.contrib import admin
#추가된 내용
from django.urls import path
import wordcount.views
#여기까지

urlpatterns = [
    url(r'^admin/', admin.site.urls),
#추가된 내용
    path('', wordcount.views.home, name="home"),
    path('about/', wordcount.views.about, name="about"),
#여기까지
]

{% endhighlight %}

URL 패턴은 아래와 같이 사용됩니다. 원래 django는 url()의 형식을 사용해 url을 연결했습니다. 그러나 admin 주소에서 보이는 것처럼 그 표현을 정규표현식으로 해야했기 때문에 가독성이 떨어진다는 문제점이 있었습니다. 그래서 Django 2 부터 path라는 기능이 새로 생겼고 path는 훨씬 쉽게 url 작성이 가능합니다. <참고: [https://docs.djangoproject.com/en/2.2/topics/http/urls/](https://docs.djangoproject.com/en/2.2/topics/http/urls/)>

path()를 작성할 때는 아래와 같은 형식으로 작성해주면 됩니다.
{% highlight python %}
path('기본 url 뒤에 붙는 상세 주소', 
    '그 주소로 접속하면 연결되는 view가 어디인지', 
    '이 url을 호출할 때 사용할 이름이 무엇인지'),
{% endhighlight %}
예를 들어, 
{% highlight python %}
path('', wordcount.views.home, name="home"),
{% endhighlight %}
이라고 한다면, "localhost:8000" 혹은 "127.0.0.1:8000" 뒤에 아무것도 안 붙였을 때, wordcount 앱 views 안에 home에 작성된 기능을 작동시키고, 이 주소를 우리는 앞으로 편의상 home이라고 부를게 라는 이야기 이고,
{% highlight python %}
path('about', wordcount.views.about, name="about"),
{% endhighlight %}
이라고 한다면, "localhost:8000/about" 혹은 "127.0.0.1:800/about"의 주소로 접속했을 때, wordcount 앱 views 안에 about 이라는 기능을 작동시키고, 이 주소를 우리가 앞으로 편의상 about이라고 부를게 라는 이야기 입니다.

---

## 05. Templates 제대로 작성하기

### 05-1. Templates 연결하기

앞서 간략하게 이름만 적어 놓았던 ".html" 파일들을 이제 제대로 한번 만들어 볼 시간입니다. 

먼저 home.html 페이지에는 about으로 연결되는 링크와 텍스트의 길이를 확인할 입력창이 필요합니다. 코드는 아래와 같습니다. 
{% highlight html %}
<a href=""> ABOUT </a>

<form action="">
    <textarea cols="40" rows="10" name="fulltext"></textarea>
    <br>
    <input type="submit" value="Count!">
</form>
{% endhighlight %}

이제 ABOUT을 누르면 about.html로 넘어가게 기능을 만들어 보겠습니다. 매우 쉽습니다.
{% highlight html %}
<a href="{%raw%}{% url 'about' %}{%endraw%}">ABOUT</a>
{% endhighlight %}
이라고 입력해주면 됩니다. 

사실,
{% highlight html %}
<a href="/about">ABOUT</a>
{% endhighlight %}
이라고 입력 해줘도 똑같이 작동합니다. 이게 우리가 공부 했던 더 익숙한 html 방식 일 것입니다. 그러나 웹 페이지 개발을 할 때 더 효율적인 방법은 첫번째 방법입니다. 

{%raw%}{% %}{%endraw%} 표시는 그 안에서는 django 문법을 불러 오겠다는 뜻입니다. 즉 url 중에서 이름이 'about' 이라는 주소를 불러 오겠다는 뜻이죠. 아까 url.py 파일에서 name="" 설정 해줬을 때의 그 값을 말합니다. 

즉 코딩 도중에 상세 주소가 바뀌더라도, 그 이름을 찾아서 가기 때문에 html 파일을 하나하나 수정해 줄 필요가 없다는 얘기입니다. 

### 05-2. 입력된 텍스트 넘기기

자 이제 실제로 글자를 카운트 하는 기능도 만들어 봅시다. 

가장 먼저 view에서 입력된 글을 처리하는 기능을 만들어줘야 합니다. 

먼저 result라는 이름의 기능을 views.py 와 urls.py 파일에 추가해줍시다. 
{% highlight python %}
#views.py
def result(request):
    return render(request, 'wordcount/result.html')

# urls.py
urlpatterns = [
...
    path('result/', wordcount.views.result, name="result"),
]
{% endhighlight %}

그리고 home.html에 접속해서 "Count!" 기능을 누르면 "count" 기능이 실행되게 설정을 해줘야 합니다. 
{% highlight html %}

<a href="{%raw%}{% url 'about' %}"{%endraw%}> ABOUT </a>

<form action="{%raw%} {% url 'result' %}"{%endraw%}>
    <textarea cols="40" rows="10" name="fulltext"></textarea>
    <br>
    <input type="submit" value="Count!">
</form>
{% endhighlight %}

이렇게 바꿔주면 됩니다. 그러나 이렇게만 해서는 기능이 작동되지 않습니다. 앞서 말했던 것 처럼 result 페이지는 단순히 어떠한 페이지를 보여주는 home, about과는 다르게 그때그때 입력된 값에 따라 다른 결과를 보여줘야 합니다. 

그래서 Count! 버튼을 누르면 form 태그 안에서 작성된 textarea의 값을 함께 가지고 result.html로 넘어갈 수 있게끔 코드를 추가해줘야 합니다. 
{% highlight python %}
#views.py
def result(request):
    input_text = request.GET['fulltext']
    return render(request, 'result.html', {%raw%}{'fulltext': input_text}{%endraw%})

input_text = request.GET['fulltext']
{% endhighlight %}

위 코드는 form 태그가 보내는 "fulltext"라는 이름을 가진 값을 input_text라는 이름을 가진 변수에 넣겠다는 뜻 입니다. 
{% highlight python %}
return render(request, 'wordcount/result.html', {%raw%}{'fulltext': input_text}{%endraw%})
{% endhighlight %}
가장 뒤에 추가된 부분은 result.html에서 fulltext라는 이름을 통해 input_text에 들어있는 변수를 불러 오겠다는 것입니다. 

이제 그 결과를 result.html 페이지에 띄울 차례 입니다. 
{% highlight html %}
<h1> Result </h1>

<h3> 당신이 입력한 텍스트는 <!--총 단어수 --> 단어로 구성되어 있습니다. </h3>
<br>
<h3> 입력한 텍스트 </h3>
{%raw%}{{ fulltext }}{%endraw%}
<br>
<h3> 단어 카운트 <h3>
<!-- '단어' : '단어 나온 횟수' -->
{% endhighlight %}
이 때 {%raw%}{{ }}{%endraw%} 는 {%raw%}{% %}{%endraw%}와 달리 django를 통해 넘겨받은 데이터를 출력할 때 사용하는 코드입니다. 

### 05-3. Count 함수 기능 추가하기

가장 먼저 총 단어 수를 보여주는 기능을 추가해 보겠습니다. 
{% highlight python %}
#views.py
    
def result(request):
    input_text = request.GET['fulltext']

    word_list = input_text.split() # input_text 변수 속에 있는 글을 띄어쓰기 기준으로 쪼개는 기능

    return render(request, 'result.html', {%raw%}{'fulltext': input_text, 'total': len(word_list)}){%endraw%}
    #total 이라는 변수에 word_list의 길이를 넣을 것이다. 
{% endhighlight %}
이제 result 페이지에 이 값을 보여주면 되겠죠?
{% highlight html %}
<!-- result.html --> 

<h1> Result </h1>

<h3> 당신이 입력한 텍스트는 {%raw%}{{total}} {%endraw%}단어로 구성되어 있습니다. </h3>
...
{% endhighlight %}
이제 각 단어별로 몇번 반복 되었는지 세는 기능을 만들어 보겠습니다. 
{% highlight python %}
#views.py

def result(request):
    input_text = request.GET['fulltext']

    word_list = input_text.split()

    word_dictionary = {%raw%}{}{%endraw%}

    for word in word_list:
        if word in word_dictionary:
        word_dicrtionary[word] += 1
        else:
        word_dictionary[word] = 1

    return render(request, 'result.html', {%raw%}{'inputtext': input_text, 'total': len(word_list), 'dictionary': word_dictionary.items()}){%endraw%}

{% endhighlight %}
코드를 차근차근 설명하자면,
{% highlight python %}
    word_dictionary = {%raw%}{}{%endraw%}
{% endhighlight %}
먼저 word_dictionary 라는 이름을 가진 딕셔너리를 하나 만들어주라는 코드 입니다.
{% highlight python %}
for word in word_list:
    if word in word_dictionary:
        word_dictionary[word] += 1
    else:
        word_dictionary[word] = 1
{% endhighlight %}
그 다음 "word_list를 word라는 이름으로 받아오고, 그 리스트에 단어가 이미 딕셔너리 key 값으로 있다면, value에 1을 더하고 / 없다면 해당 이름으로 key 값을 만들고 그 value는 1로 해라" 라는 의미의 코드입니다.
{% highlight python %}
return render(request, 'result.html',{%raw%} {'fulltext': input_text, 'total': len(word_list), 'dictionary': word_dictionary.items()}){%endraw%}
{% endhighlight %}
그리고 word_dictionary의 모든 값을 dictionary 라는 이름의 변수로 result.html에서 보여주겠다는 뜻입니다. 

이제 views.py 작성이 끝났으니 result.html로 가서
{% highlight html %}
<!-- result.html --> 

<h1> Result </h1>
<h3> 당신이 입력한 텍스트는 {%raw%}{{total}} {%endraw%}단어로 구성되어 있습니다. </h3>

<h3> 입력한 텍스트 </h3>
{%raw%} {{ fulltext }} {%endraw%}
<br>
<h3> 단어 카운트 <h3>

{%raw%} {% for word, frequency in dictionary %}
{{word}} - {{frequency}}
<br>
{%endfor%}
{%endraw%}
{% endhighlight %}
라고 입력해줍니다. 

이를 해석해보면 dictionary의 내용을 하나하나씩 가져와서 key 값은 word라고 부르고, value 값은 frequency라고 부를 것이다. 그리고 순서대로 {%raw%}{{word}} - {{frequency}} {%endraw%}형태로 출력할 것이다, 라는 뜻입니다. 

---

# Parsing

이번에는 파싱에 대해서 배워보겠습니다. 파싱은 웹사이트에서 우리가 원하는 정보를 추출해오는 것을 말합니다. 네이버 실시간 검색어을 뽑아서 보여주거나, 날씨정보를 가져와서 보여줄 수 있죠. 웹 개발을 할 때 다양하게 접목 될 수 있는 기술입니다. 

오늘은 간단하게만 파싱에 대해서 배워보도록 하겠습니다. 먼저는, Django 프레임워크를 사용하지 않고 그냥 파이썬 파일을 생성해서 결과를 출력 해보겠습니다. 

그래도 뭔가 우리와 관련이 있는 정보를 가져와보는 실습을 하는게 좋을 것 같아서, 학교 홈페이지 정보를 가져와 보려고 합니다. 

먼저, 가상환경을 켜주시고, 그 안에 crawl.py라는 이름의 파일을 생성해주시면 됩니다. 

그 후 패키지 두 개를 설치해 주시면 됩니다. 
{% highlight bash %}
pip install requests
pip install beautifulsoup4
{%endhighlight%}
그 후 crawl.py에 두 기능을 모두 import 해줍니다. 코드는 아래와 같습니다. 
{% highlight python %}
from urllib.request import urlopen
from bs4 import BeautifulSoup
{%endhighlight%}
그러면 이제 크롤링을 할 준비가 된 것 입니다. 

urlopen은 말 그대로 특정 url에 접근 가능하게 해주는 기능이고, beautifulsoup는 해당 url에서 우리가 가져오고 싶은 정보를 가져오게 해주는 패키지 입니다. 

먼저 간단하게 페이지를 가져오는 기능을 실행해 보겠습니다. 
{% highlight python %}
from urllib.request import urlopen
from bs4 import BeautifulSoup

#html이라는 변수를 지정한 후, 거기 안에 특정 url 주소를 입력해준다.
html = urlopen("http://builder.hufs.ac.kr/user/indexSub.action?codyMenuSeq=37080&siteId=hufs&menuType=T&uId=4&sortChar=AB&linkUrl=04_0202.html&mainFrame=right")
#beautifulsoup를 이용해서 해당 페이지를 파싱해온다.
bsObject = BeautifulSoup(html, "html.parser")
#해당 페이지 정보를 프린트 해보기
print(bsObject)
{% endhighlight %}
이후 터미널에 
{% highlight bash %}
$ python3 crawl.py 
{% endhighlight %}
이라고 입력하면 해당 정보가 불러와 지는 것을 볼 수 있습니다. 

자, 이제 여기서 원하는 정보를 가공하는 방법을 알려드리겠습니다. 

먼저 특정 html 태그를 불러오는 방법입니다. 
{% highlight python %}
from urllib.request import urlopen
from bs4 import BeautifulSoup

#html이라는 변수를 지정한 후, 거기 안에 특정 url 주소를 입력해준다.
html = urlopen("http://builder.hufs.ac.kr/user/indexSub.action?codyMenuSeq=37080&siteId=hufs&menuType=T&uId=4&sortChar=AB&linkUrl=04_0202.html&mainFrame=right")
#beautifulsoup를 이용해서 해당 페이지를 파싱해온다.
bsObject = BeautifulSoup(html, "html.parser")
#해당 페이지에서 table 태그 불러오기
table = bsObject.find("table")

print(table)
{% endhighlight %}
이렇게 실행하면 table 안에 있는 정보만 불러 오는 기능을 실행합니다. 

만약 공지사항에서 글 제목을 끌고 오려면 어떻게 해야할까요?
{% highlight python %}
from urllib.request import urlopen
from bs4 import BeautifulSoup

#html이라는 변수를 지정한 후, 거기 안에 특정 url 주소를 입력해준다.
html = urlopen("http://builder.hufs.ac.kr/user/indexSub.action?codyMenuSeq=37080&siteId=hufs&menuType=T&uId=4&sortChar=AB&linkUrl=04_0202.html&mainFrame=right")
#beautifulsoup를 이용해서 해당 페이지를 파싱해온다.
bsObject = BeautifulSoup(html, "html.parser")
#해당 페이지에서 table 태그 불러오기
table = bsObject.find("table")
#불러온 table 태그에서 class를 title로 가지고 있는 모든 정보 가져오기
titles = table.find_all(class_="title")
#불러온 타이틀들에서 각각,
for title in titles:
    #태그들을 제외한 텍스트만을 가져와라
    x = title.get_text()
    print (x)
{% endhighlight %}

이렇게 하면 쭉 제목만 뽑아 올 수 있습니다. 다만 저희 학교 페이지의 경우 많은 공백이 포함되어 있습니다. 이를 다양한 파이썬 문법을 사용해 해결 할 수 있겠지만, 저의 경우 아래와 같이 하는 것이 가장 깔끔하게 정보를 보여줄 수 있었습니다. 
{% highlight python %}
from urllib.request import urlopen
from bs4 import BeautifulSoup

#html이라는 변수를 지정한 후, 거기 안에 특정 url 주소를 입력해준다.
html = urlopen("http://builder.hufs.ac.kr/user/indexSub.action?codyMenuSeq=37080&siteId=hufs&menuType=T&uId=4&sortChar=AB&linkUrl=04_0202.html&mainFrame=right")
#beautifulsoup를 이용해서 해당 페이지를 파싱해온다.
bsObject = BeautifulSoup(html, "html.parser")
#해당 페이지에서 table 태그 불러오기
table = bsObject.find("table")
#불러온 table 태그에서 class를 title로 가지고 있는 모든 정보 가져오기
titles = table.find_all(class_="title")
#불러온 타이틀들에서 각각,
for title in titles:
    #태그들을 제외한 텍스트만을 가져와라
    x = title.get_text()
    x = x.replace("\t", '')
    x = x.replace("\n", '')
    print(x)
{% endhighlight %}
업그레이드를 조금 더 해보고자 합니다. 해당 공지글 제목과, 공지글의 url을 같이 가져와 보겠습니다. 
{%highlight python %}
from urllib.request import urlopen
from bs4 import BeautifulSoup

#html이라는 변수를 지정한 후, 거기 안에 특정 url 주소를 입력해준다.
html = urlopen("http://builder.hufs.ac.kr/user/indexSub.action?codyMenuSeq=37080&siteId=hufs&menuType=T&uId=4&sortChar=AB&linkUrl=04_0202.html&mainFrame=right")
#beautifulsoup를 이용해서 해당 페이지를 파싱해온다.
bsObject = BeautifulSoup(html, "html.parser")
#해당 페이지에서 table 태그 불러오기
table = bsObject.find("table")
#불러온 table 태그에서 class를 title로 가지고 있는 모든 정보 가져오기
titles = table.find_all(class_="title")
#불러온 타이틀들에서 각각,
for title in titles:
    #태그들을 제외한 텍스트만을 가져와라
    x = title.get_text()
    x = x.replace("\t", '')
    x = x.replace("\n", '')
    print(x)
    
    #링크라는 변수를 만들어서, 그 안에 a태그의 href정보를 가져와라
    link = title.a.get('href')
    #학교 기본 url에 뽑아온 링크를 합쳐라
    url = 'http://www.hufs.ac.kr/user/' + link
    print(url)
{% endhighlight %}
이렇게 되면 글 제목 밑에 페이지 url이 나오게 됩니다. 클릭해서 해당 제목을 가진 글로 잘 연결되는지 확인 해보시기 바랍니다.

이제 이걸 Django에서 어떻게 사용할 수 있는지 보여드리겠습니다. 전체 웹을 다시 만들기는 조금 번거로우니 아까 만들었던 워드카운트 앱 about 페이지에 임시로 한번 이식했다가 지워봅시다. 

먼저 views.py를 켜줍니다.

"about" 아래에 위에 코드를 붙여 넣어줍니다.
{% highlight python %}
#views.py

def about(request):
    from urllib.request import urlopen
    from bs4 import BeautifulSoup

    html = urlopen("http://builder.hufs.ac.kr/user/indexSub.action?codyMenuSeq=37080&siteId=hufs&menuType=T&uId=4&sortChar=AB&linkUrl=04_0202.html&mainFrame=right")
    bsObject = BeautifulSoup(html, "html.parser")
    table = bsObject.find("table")
    titles = table.find_all(class_="title")
    for title in titles:
        x = title.get_text()
        x = x.replace("\t", '')
        x = x.replace("\n", '')
        link = title.a.get('href')
        url = 'http://www.hufs.ac.kr/user/' + link
    return render(request, 'about.html')
{% endhighlight %}
앞서 Wordcount 에서 Result를 가져온 것과 비슷한 방식으로 해당 페이지도 구성하고자 합니다. 

먼저 Dictionary를 하나 만들어줍니다.
{% highlight python %}
{%raw%}
#views.py
{%endraw%}
def about(request):
    from urllib.request import urlopen
    from bs4 import BeautifulSoup
    {%raw%}
    html = urlopen("http://builder.hufs.ac.kr/user/indexSub.action?codyMenuSeq=37080&siteId=hufs&menuType=T&uId=4&sortChar=AB&linkUrl=04_0202.html&mainFrame=right")
    {%endraw%}
    bsObject = BeautifulSoup(html, "html.parser")
    table = bsObject.find("table")
    titles = table.find_all(class_="title")
    #딕셔너리 만들기
    notices = {%raw%}{}{%endraw%}
    for title in titles:
        x = title.get_text()
        x = x.replace("\t", '')
        x = x.replace("\n", '')
        link = title.a.get('href')
        url = 'http://www.hufs.ac.kr/user/' + link
    return render(request, 'about.html')
{% endhighlight %}
그리고 이제 학교 공지글의 제목을 Key에, 해당 공지글 링크를 Value에 넣어봅시다. 
{% highlight python %}
#views.py

def about(request):
    from urllib.request import urlopen
    from bs4 import BeautifulSoup
{%raw%}
    html = urlopen("http://builder.hufs.ac.kr/user/indexSub.action?codyMenuSeq=37080&siteId=hufs&menuType=T&uId=4&sortChar=AB&linkUrl=04_0202.html&mainFrame=right"){%endraw%}
    bsObject = BeautifulSoup(html, "html.parser")
    table = bsObject.find("table")
    titles = table.find_all(class_="title")
    notices = {%raw%}{}{%endraw%}
    for title in titles:
        x = title.get_text()
        x = x.replace("\t", '')
        x = x.replace("\n", '')
        link = title.a.get('href')
        url = 'http://www.hufs.ac.kr/user/' + link
        #딕셔너리에 자료 추가
        notices[x] = url
    return render(request, 'about.html')
{% endhighlight %}
마지막으로 해당 딕셔너리를 Templates로 넘겨줍니다.
{% highlight python %}
#views.py

def about(request):
    from urllib.request import urlopen
    from bs4 import BeautifulSoup
    {%raw%}
    html = urlopen("http://builder.hufs.ac.kr/user/indexSub.action?codyMenuSeq=37080&siteId=hufs&menuType=T&uId=4&sortChar=AB&linkUrl=04_0202.html&mainFrame=right")
   {%endraw%}   
    bsObject = BeautifulSoup(html, "html.parser")
    table = bsObject.find("table")
    titles = table.find_all(class_="title")
    notices = {%raw%}{}{%endraw%}
    for title in titles:
        x = title.get_text()
        x = x.replace("\t", '')
        x = x.replace("\n", '')
        link = title.a.get('href')
        url = 'http://www.hufs.ac.kr/user/' + link
        notices[x] = url
    #Templates로 가져가기
    return render(request, 'about.html', {'notices': notices.items()})
{% endhighlight %}
자 이제 about.html 파일로 갑니다. 
{% highlight html %}
<!-- about.html -->
<h1> About Wordcount </h1>
{%raw%}
{% for title, link in notices%}
{{title}} - {{link}}
{%endfor%}
{%endraw%}
{% endhighlight %}
이렇게 입력해 준 후, 서버를 키고 about 페이지에 접속해 봅니다.

이런식으로 나오면 성공적으로 잘 하신 겁니다.

다만, 이렇게 되면 가독성도 떨어지고 글로 찾아가기도 어려우니 직접 링크를 만들어서 달아보도록 하겠습니다.
{% highlight html %}
<!-- about.html -->
<h1> About Wordcount </h1>
{%raw%}
{% for title, link in notices%}
{{title}} <a href="{{link}}">글 보기</a>
<br>
{%endfor%}
{%endraw%}   
{%raw%}
<!-- about.html -->
<h1> About Wordcount </h1>
{% for title, link in notices%}
<a href="{{link}}">{{title}}</a>
<br>
{%endfor%}
{%endraw%} 
{% endhighlight %}
위에 두 방법 중 더 마음에 드시는 방법으로 사용하시면 되겠습니다. 

이렇게 BeautifulSoup을 이용한 파싱의 기본을 배워보았습니다.

예쁘게 꾸미시는건 여러분의 몫으로 남기겠습니다! 

### 고생하셨습니다! ✌️
