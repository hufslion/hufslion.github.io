---
layout: post      # 수정 NO 
title: 예비세션_승윤 # 글 제목으로 수정
category: Session # 본인에게 맞는 카테고리명을 적으세요
--- 

 예비 세션: 파이썬으로 네이버 뉴스 크롤링 하기.
 
 # 네이버 실시간 뉴스 크롤링 및 단어 세어보기.
 
 ---
<br>


&nbsp;**1. 크롤링이란?.**
인터넷 주소로 서버에 데이터를 요청해서 받아오는 것
* 라이브러리 : urlopen
* 라이브러리의 의미와 urlopen 라이브러리의 기능 소개 및 설명

&nbsp;**2. 파싱이란?.**
크롤링한 데이터에서 값을 뽑아내는 것
* 라이브러리 bs4 BeautifulSoup 
* BeaurifulSoup의 기능 소개 및 설명

&nbsp;**3. 프로젝트 및 크롤링결과 표출할 기본 앱과 페이지 생성하기.**
```console 
  python –m venv myvenv
  
  source myvenv/Scripts/activate
  
  pip install django  
  
  django-admin startproject crawlingsession
  
  cd crawlingsession
  
  python manage.py startapp crawlingsessionapp
```
* setting.py 파일에 만든 앱 폴더 등록하기
* app폴더 templates 폴더 생성하고 home.html 파일 만들기
* views 파일에 앱 임포트후 home 함수 추가 및 url 설정하기.

&nbsp;**4. bs4 BeautifulSoup 설치하기.**
```console 
  pip install bs4  
```
&nbsp;**5. views 파일에 rullib 라이브러리와 설치한 BeautifulSoup 임포트하기.**
```python

import urllib.request
from urllib.request import urlopen
import bs4

```

&nbsp;**6. 네이버 뉴스 페이지 소스 긁어오기.**
```python

from django.shortcuts import render

import urllib.request
from urllib.request import urlopen
import bs4

def home(request):
    
    url = "https://news.naver.com/"
    html = urlopen(url)

    
    bsobj = bs4.BeautifulSoup(html.read(), "html.parser")

    return render(request, 'home.html', {'show':bsobj})
    
```

* 코드 설명(현장).
* 실시간 뉴스 제목 검색해서 뜨는지 확인해 보기.

&nbsp;**7. 특정 id 블럭의 정보 뽑아내기.**

* 네이버 뉴스 --> 개발자 도구를 통해서 페이지 소스 분석하기 및 설명
* bs4의 find 기능을 활용하여, 뽑아내고자 하는 id 블럭 찾아내기.
* 이번에는 실시간 분야별 메인 뉴스를 뽑아낸다.

```python
    bsobj2 = bsobj.find("div", {"id":"main_content"})
```

* bsobj2를 넘겨서 출력해보면, 실시간 주요 뉴스의 정보만 뽑아져 나오는 것을 확인 할 수 있음.

&nbsp;**8. 뉴스제목만 뽑아내기.**

* 뉴스 제목만 뽑아내기 위해, 네이버 뉴스 페이지소스에서 제목이 어떠한 태그 블록에 있는지 찾아보기.
* 네이버 뉴스 제목들이  strong 태그들에 포함 되어있는것 확인하기.
* stong 에 포함되어있는 제목들만 뽑아내기.

```python
     from django.shortcuts import render

import urllib.request
from urllib.request import urlopen
import bs4

def home(request):
    
    url = "https://news.naver.com/"
    html = urlopen(url)
    bsobj = bs4.BeautifulSoup(html.read(), "html.parser")
    bsobj2 = bsobj.find("div", {"id":"main_content"})

    title = bsobj2.findAll("strong")

    return render(request, 'home.html', {'show':title})
```
* 하지만, 리스트마다 strong 태그까지 모두 뽑아온 것을 확인 할 수 있다.

&nbsp;**9. 뽑아낸 제목을 처리하기 쉽도록 strong 태그 없애주기.**
* bs4의 text 클래스를 활용.
```python 
from django.shortcuts import render

import urllib.request
from urllib.request import urlopen
import bs4

def home(request):
    
    url = "https://news.naver.com/"
    html = urlopen(url)

    
    bsobj = bs4.BeautifulSoup(html.read(), "html.parser")

    bsobj2 = bsobj.find("div", {"id":"main_content"})

    title = bsobj2.findAll("strong")

    title_len = len(lis)
    for i in range(title_len):
        title[i] = title[i].text



    return render(request, 'home.html', {'show':title})
 ```
* 코드 설명하기. (리스트 길이 len, for문 range, title[] 리스트 개념, bs4의 text 기능)
* strong태그가 사라진 것 확인.
* 제목을 보기 쉽게 for문을 써서 다시 출력해 보자. (home.html파일)
```html
{%for title in show%}

기사 제목 : {{title}}<br><br>

{%endfor%} 
```

# 추가 내용(wordcount 세션 이후 --> default dictionary 활용하여 뉴스 제목 단어 세어보기).
 ---
* default dictionary 기능을 활용하여 
&nbsp;**1. 불러온 제목의 단어를 세어보자.**
*default dictionary 임포트하기.
```python
from collections import defaultdict
```
* default dictionary 기능 설명.

* 리스트를 한개의 문자열로 모두 합치기.
```python
    one_list = ' '.join(title)
```

* default dictionary 호출하여 단어 갯수를 담을 사전 초기화하기.
```python
    d=defaultdict()
    d=defaultdict(lambda:0)
```

* 단어 쪼개서 갯수 세기.

```python 
words = one_list.split(" ")
    
    for i in words:
        d[i] = d[i]+1
```

* item 함수로 사전 넘기기. (전체코드)
```python
from django.shortcuts import render

from collections import defaultdict
import urllib.request
from urllib.request import urlopen
import bs4

def home(request):
    
    url = "https://news.naver.com/"
    html = urlopen(url)

    
    bsobj = bs4.BeautifulSoup(html.read(), "html.parser")

    bsobj2 = bsobj.find("div", {"id":"main_content"})

    title = bsobj2.findAll("strong")

    title_len = len(title)
    for i in range(title_len):
        title[i] = title[i].text
    
    one_list = ' '.join(title)

    d=defaultdict()
    d=defaultdict(lambda:0)

    words = one_list.split(" ")
    
    for i in words:
        d[i] = d[i]+1

    return render(request, 'home.html', {'show':title, 'num_c':d.items()})
 ```
 
* 단어 갯수 출력하기.
 ```html
<h1>실시간 뉴스</h1>
{{show}}

{%for title in show%}

기사 제목 : {{title}}<br><br>

{%endfor%} 

<br>
{% for word, number in num_c %}
    {{word}} : {{number}} 개
<br>
{%endfor%}

 ```
 
 &nbsp;**네이버 뉴스에는 겹치는 내용의 뉴스가 없기 때문에, 단어를 세는것은 의미가 없다. 하지만, 다른 포털사이트나, 게시판에서 활용할 수 있음.**





