---
layout: post
title: 1. 웹개론과 html
category: session
---
세션 1 - prod. 이슬

# 웹 개론

### 웹 서비스란?
- **서비스** : 고객이 요청을 하여, 그에 맞는 응답을 제공하는 것!  
- 웹 서비스 또한, 서비스와 같습니다.  
- 고객(client)이 요청(request)을 하고, 서버(server)가 응답(response)를 하는 것이죠!  
- 이 때, 이 관계를 client - server 구조라고 합니다!  

> 웹 서비스를 만든다!
  
- 라고 한다면, 우리는 서버(server)의 관점으로 생각해야겠죠?!  
  
### Request?
- 요청(request)에는 여러 가지 방식이 있습니다. 그 중 두 가지를 살펴보도록 합시다!  
- **GET** : 무언가를 가져다 줘!  
- **POST** : 무언가를 처리해 줘!  
- 이 때, 고객(client)이 요청을 바라는 이 무언가는 무엇일까요?  
- 바로 HTML 파일입니다!! 사실, HTML 파일 외에도 여러 가지가 있지만 일단은!! HTML 파일이라고 알아둡시다!  

### 웹 개발
- 웹 개발은 크게 프론트엔드와 백엔드로 나뉩니다.  
- **Frontend**: 이용자의 눈에 보이는 부분  
- **Backend** : 이용자의 눈에 보이지 않는 부분  
- **마크업 언어** : 프론트엔드에서 사용하는 언어로써 html, css, js 등이 있습니다.  
- **프로그래밍 언어** : 백엔드에서 사용하는 언어로써 ruby on rails, python django, php 등이 있습니다.  
  
### 웹 브라우저
- '웹 서버에서 상방향 통신하는 html 문서나 파일과 연동하고 출력하는 응용 소프트웨어'라고 정의합니다.  
- 흔히 알고 있는 인터넷 익스플로러, 크롬, 파이어폭스가 예시에 속합니다.  
- 유용한 도구들이 많은 **크롬**을 사용하는 것을 추천합니다.  
  

# html 실습
### html 구성
- html은 크게 글, 태그, 속성 세 가지로 구성됩니다.  
- **글** : 전달하고자 하는 내용입니다.  
- **태그** : 의미를 지어주기 위해 붙이는 식별자  
- **속성** : 태그에 적용되는 효과, 구체적 표현 정보  


### 프로젝트를 만들 폴더 생성하기
{% highlight html %}
$ mkdir basic
{% endhighlight %}

### index.html 파일 만들기
### html로 작성된 문서입니다! 라고 알려주는 태그
{% highlight html %}
<!DOCTYPE html>
<html lang="ko"> 


</html>
{% endhighlight %}
- "ko" 는 한글로 되어 있다고 알려줌!  

### 화면에 직접 드러나지 않지만 문서를 설명해주는 태그
{% highlight html %}
<head>
  <meta charset="utf-8">  
  <title> html이 뭘까?</title> 
</head>
{% endhighlight %}
- html 태그 안에 적어줍니다.  
- "utf-8"은 인코딩 방식을 알려줍니다.  
- title 태그는 문서에 드러나지 않지만 한마디로 페이지를 설명해주는 태그입니다!  
어떻게 설명해주는 지 코드를 실행했을 때를 살펴보세요!  


### 화면에 직접적으로 등장하는 태그
{% highlight html %}
<body>
이게 보일까요?!
</body>
{% endhighlight %}
- a, img, h1, li 등등 여러가지 태그들이 있습니다.  
- html은 정형화된 문법으로 반복적으로 사용되기 때문에 태그들을 익혀두면 사용하기 쉽습니다.  

### 기본 틀을 만들어주는 단축키
{% highlight html %}
!+tab
{% endhighlight %}
  
- 기본적인 틀을 만들어주는 단축키


### 이 body 태그 안 부분을 여러 태그들로 한 번 꾸며볼까요?
### h1 태그 - 중요 제목을 나타내는 태그
{% highlight html %}
<h1> 안녕하세요 </h1>
<h2> 멋쟁이 사자가 되고 싶어요! </h2>
<p> 저는 한국외국어대학교 바이오메디컬공학부 이슬입니다.
띄어쓰기가 언제 될까요?<br>
이제 될까요?! </p>
{% endhighlight %}
- h1 ~ h6 까지 있으며, 숫자가 작은 순서대로 더 중요한 제목을 나타냅니다.  
- 일반 단락을 나타내는 태그는 p 태그입니다.  
- br 은 단락 바꿈을 해주는 태그입니다.  
  
### form 태그 - 입력값을 받아들이는 태그
{% highlight html %}
<form action ="전송받을 대상"> 
    아이디 : <input type="text" name="id">
    비밀번호 : <input type="password" name="pw">
    <input type="submit">
</form>
{% endhighlight %}
- 사용자들로부터 입력받은 태그들을 전송받을 대상에게 action으로 전송합니다.  
  
### img 태그 - 이미지를 첨부하는 태그
{% highlight html %}
<img src=".jpg" width=100>
{% endhighlight %}
  
- 이미지를 첨부하는 태그로 높이나 너비를 조절할 수 있습니다.

### ol 태그 - 순서가 있는 리스트를 만들어주는 태그
{% highlight html %}
<ol>
  <li>1학년</li>
  <li>2학년</li>
  <li>3학년</li>
  <li>4학년</li>
 </ol>
{% endhighlight %}
- ordered list 태그 : 순서가 있는 리스트들을 만들어 줍니다.  
- li 태그 : 리스트 항목들을 적어줍니다.  
- 단축키 : ol>li*개수 + tab  

### ul 태그 - 순서가 없는 리스트를 만들어주는 태그
{% highlight html %}
<ul>
  <li>여름 계절학기</li>
  <li>겨울 계절학기</li>
</ul>
{% endhighlight %}
- unordered list 태그 : 순서가 없는 리스트들을 만들어 줍니다.  
- li 태그 : 리스트 항목들을 적어줍니다.  
- 단축키 : ul>li*개수 + tab  

### a 태그 - 링크를 걸어주는 태그
{% highlight html %}
<a href="1.html">1학년</a>
{% endhighlight %}
- 링크를 걸어주는 태그입니다.  
  
  

# css
### css inline style sheet
{% highlight html %}
<h2 style = "color:blue">멋쟁이 사자가 되고 싶어요! </h2>
{% endhighlight %}

- 직접 html 태그의 style 속성에 CSS 코드를 넣어 적용시키는 방법입니다.  
처음 적용시킬 때는 편하지만, 후에 복잡한 프로젝트를 할 때 꾸미는 데에 한계가 있으며 재사용이 불가능합니다.

### css internal style sheet
{% highlight html %}
<style>
  h1 {
    color: red;
  }
</style>
{% endhighlight %}
- html 문서 안에 스타일 코드를 넣는 방법입니다.  
style 태그 안에 CSS 코드를 넣습니다. 보통 head 태그 안에 넣으나 어디에 넣어도 잘 적용이 됩니다.  
하지만, 서로 다른 html 문서에 적용할 수 없다는 단점이 있습니다.

### css linking style sheet
{% highlight html %}
p {
color: yellow;
}

<link rel="stylesheet" href="style.css">
{% endhighlight %}
- 별도의 CSS 파일을 만들고 html 문서와 연결하는 방법입니다.  
style.css를 적용시키고 싶은 html 문서에 링크만 걸어주면 되기 때문에 여러 html 문서에 적용시킬 수 있다는 장점이 있습니다.  
  

# ppt 참고하기
* * *
[1주차 세션 ppt 다운](https://github.com/16LeeSeul/7-/files/2928021/1st_session.pdf)

  
