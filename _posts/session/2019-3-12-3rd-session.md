---
layout: post 
title: 3. 파이썬과 부트스트랩 + Git
category: session
---
세션 3 - prod. 최효진

파이썬 기본문법, 부트스트랩 사용하기, 깃헙페이지 만들기

# 파이썬 기본문법1

>python 환경 다운로드 받기

<https://www.python.org/downloads/>
(python 3.7.2 최신버전 다운로드)


## 1. 변수

* 변수는 값을 저장합니다.(엄격히 말하면, 파이썬 객체의 참조주소(reference)또는 식별자(id)를 저장합니다. 그러나, 일단 편의상 값을 저장하는 것으로 생각하는 것이 좋습니다.)
* = 기호의 왼쪽에는 변수가 나오고, 오른쪽에는 표현식(expression)또는 값이 나타납니다.
* =기호는 수학의 등호과는 아무 상관이 없습니다.

{% highlight html %}
>>> lion = 7
{% endhighlight %}

여기서 lion이 변수가 되고, 'lion이 7이라는 값을 저장한다.' 라는 뜻이다.

## 2. print()문과 input()문

### 2.1 print()
출력의 역할을 담당하는 것은 print()함수 입니다.

{% highlight html %}
>>> print('Hello, Python world!')
Hello, Python world!
>>> print(2)
2
{% endhighlight %}
* print()함수는 ()안에 있는 것을 출력해주는 역할을 합니다.
* print()를 쓸 때는 꼭 ()괄호를 사용해줘야하고, 그 안에 출력하고자 하는 값을 넣습니다.

### 2.2 input()
출력의 역할을 담당하는 것이 print()라면, 입력의 역할을 담당하는 것은 input()함수 입니다.

* input()함수를 이용하여 사용자의 입력을 받을 수 있습니다. 
* input함수는 Enter키를 치기전의 모든 문자들을 하나의 문자열로 반환합니다.

{% highlight html %}
>>> a = input()
|life is too short, you need python #우리가 입력하는 곳
>>> a
life is too short, you need python
{% endhighlight %}

이와 같이 input()함수를 쓰면 입력할 수 있고, 입력한 것을 변수에 저장하여 해당 변수를 불렀을 때 우리가 입력한 것이 나오게됩니다.
(|표시는 깜박이는 커서 표시입니다.)

## 3. 자료형

### 3.1 숫자 자료형(int와 float)

* 정수형 자료형(int)
2, 50, 69, 34, 333333444444444

* 실수형 자료형(float)
3.141592, 4.19, 5.6007

>type() : 변수의 자료형을 알 수 있는 방법 => type이라는 함수안에 변수를 입력하면, 변수의 자료형이 나온다.


{% highlight html %}
>>>my_int = 3
>>>type(my_int)
<class 'int'>
	
>>>my_float=3.14
>>>type(my_float)
<class 'float'>
{% endhighlight %}

이와 같이 자료형을 파악할 수 있습니다.

>숫자의 사칙연산

{% highlight html %}
>>> 1+2
3
>>> 4-3
0
>>> 5*20
100
>>> print(3//2)
#결과:
>>> print(3/2)
#결과:
{% endhighlight %}

* print(3//2)의 결과는 무엇일까요?
	답은 1입니다.파이썬에서 //는 정수나누기를 하고, 소수점 뒤에 있는 부분을 버립니다.
* print(3/2)의 결과는 무엇일까요?
	답은, 1.5입니다. 파이썬에서 /는 실수나누기를 하고 만약에 print(3/1)을 했다고 하면 답이 3 이 아니라, 3.0과 같이 실수 타입으로 변환시켜서 출력합니다.
	
### 3.2 문자열(string)

{% highlight html %}
>>> "Hello"
'Hello'
>>> 'Hello'
'Hello'
>>> my_string= "I'm lion"
>>> my_string
I'm lion
{% endhighlight %}

* 문자열은 큰따옴표"" 나 작은따옴표''로 묶어줍니다.
* 큰따옴표와 작은따옴표를 섞어쓰면 안됩니다.

>문자열의 결합
+연산자를 통해서 문자열을 결합시킬 수 있습니다.


{% highlight html %}
>>> first_name= 'lion'
>>> last_name='like'
>>> full_name = last_name + ' ' + first_name
>>> print(full_name)
'like lion'
{% endhighlight %}

### 3.3 Boolean
* 참과 거짓. 예, 아니오. True, False
* 비교하거나 논리연산자를 쓸 때 많이 사용.

{% highlight html %}
>>> my_bool = True
>>> print(my_bool)
True
	
>>> 3!=5
True
>>> 3>5
False
{% endhighlight %}

### 3.4 리스트(list)

{% highlight html %}
[val1, val2, ..]
{% endhighlight %}

* 리스트는 변수에 저장되는 항목(item)의 모음입니다.
* 리스트에 저장할 수 있는 항목에는 제한이 없지만, 항목은 서로 관련된 것으로 구성되어야 합니다.

{% highlight html %}
>>> my_list = []
>>> my_list = [1, 2, 3] #숫자를 저장할 수 있다.
>>> students = ['hyojin', 'eunjin', 'kijung'] #문자열도 저장할 수 있다.
{% endhighlight %}

* 리스트의 각 항목은 (0으로 시작하는)위치로 식별됩니다.(**1이 아닌 0부터 시작**)
* 리스트의 첫번째 요소를 액세스 하려면 리스트 이름을 쓰고 그 뒤에 괄호안에 0을 쓰면됩니다.

{% highlight html %}
>>> students[0]
'hyojin'
>>> students[1]
'eunjin'
>>> students[-1]
'kijung'
{% endhighlight %}

* 이와 같이 괄호 안의 숫자를 항목의 색인(index)라고 합니다.
* 리스트는 0부터 시작하기 때문에 항목의 색인은 항상 리스트의 위치보다 하나 작습니다. 따라서 리스트에서 두번째 항목을 얻으려면 색인 1을 사용해야 합니다.
* 리스트의 길이가 3이라면 마지막 항목을 얻으려면 색인 2를 사용할 것입니다.
* 하지만 리스트의 길이에 상관없이 리스트의 마지막 항목을 가져오려면 색인 -1을 사용하는 것이 좋습니다.

### 3.5 튜플(tuple)

{% highlight html %}
(val1, val2,…)
{% endhighlight %}

* 튜플(tuple)은 기본적으로 변경할 수 없는(immutable)리스트입니다. 
* 반면에 리스트는 매우 동적(dynamic)입니다. 리스트에서 원하는 요소를 수정할 수 있습니다.
* 하지만 튜플은 리스트와 비슷하나 안에 있는 값들을 바꿀 수 없습니다. 때로는 사용자나 프로그램이 리스트를 변경할 수 없도록할 필요가 있기 때문에 튜플이 필요합니다.
* 즉, 기술적으로 리스트는 변경가능(mutable)객체이고, 튜플은 변경불가능한(immutable)객체입니다.

{% highlight html %}
>>> students[0] = 'seul'
#위에서 했던 students리스트에서 첫번째 인자를 seul로 바꾸면
>>> students
['seul', 'eunjin', 'kijung'] #이와 같이 리스트에서는 첫번째 인자가 바뀐다.
{% endhighlight %}


{% highlight html %}
>>> my_tuple=('strawberry', 'banana', 'apple')
>>> my_tuple[0] = 'grape'
TypeError: 'tuple' object does not support item assignment
#하지만 튜플의 인자를 바꾸려고 하면, 이와 같이 에러가 난다.
{% endhighlight %}

* 튜플에서 요소를 제거하거나 요소를 추가하려 할 때도 오류가 발생한다. 일단, 튜플이 정의되면 그 튜플의 값은 변경되지 않습니다.

{% highlight html %}
>>> colors=('red', 'green', 'blue')
>>> colors.append('purple')
AttributeError: 'tuple' object has no attribute 'append'
{% endhighlight %}

* 반면에, 리스트는 값의 수정, 추가, 제거가 모두 가능하다.

{% highlight html %}
>>> colors=['red', 'green', 'blue']
>>> colors.append('purple')
>>> colors
['red', 'green', 'blue', 'purple']
{% endhighlight %}

참고로, append는 리스트에 요소를 추가시켜주는 함수입니다.

### 3.6 딕셔너리(Dictionary)

{% highlight html %}
{key1: val1, ..}
{% endhighlight %}

* 딕셔너리는 관련된 정보를 서로 연결시켜 놓은 것.
* 딕셔너리는 Key와 Value라는 것을 한 쌍으로 갖는 자료형
* 딕셔너리는 리스트나 튜플처럼 순차적으로(sequential) 해당 요소값을 구하지 않고 **Key를 통해 Value를 얻는다.**

{% highlight html %}
>>> my_dict = {'hyojin':'woman', 'kijung': 'man'}
>>> my_dict['hyojin']
'woman'
>>> grade = {'hyojin': 10, 'kebin': 99} 
>>> grade['hyojin'] 
10 
>>> grade['kebin'] 
99
{% endhighlight %}

* 'hyojin'라는 Key의 Value를 얻기 위해 my_dict['hyojin']를 사용한 것처럼 어떤 Key의 Value를 얻기 위해서는 **딕셔너리변수명[Key]**를 사용한다. (예:grade['hyojin'])

{% highlight html %}
>>> a = {1:'like', 1:'lion'} 
>>> a 
{1: 'lion'}
{% endhighlight %}

* 먼저 딕셔너리에서 Key는 고유한 값이므로 중복되는 Key 값을 설정해 놓으면 하나를 제외한 나머지 것들이 모두 무시된다는 점을 주의해야 한다.
  위의 예에서 볼 수 있듯이 동일한 Key가 2개 존재할 경우 1:'like'라는 쌍이 무시된다.

>여기서 부터는, 편의성을 위해 visual studio code에 폴더를 하나만들고, 그 안에 practice.py파일을 만들어 실행하겠습니다.


## 4. 조건문

### 4.1 if문
* 모든 if문은 True또는 False로 평가된다.
* ~가(조건문이) 맞다면 다음 문장을 실행해라.

{% highlight html %}
if 조건문: 
    수행할 문장1
    수행할 문장2
    수행할 문장3
{% endhighlight %}

'likeion이 7이라면, 맞습니다'를 출력해라. 라는 것은 if문으로 표현하면

{% highlight html %}
likelion=7
if likelion==7:
      print('맞습니다')
{% endhighlight %}

$ python practice.py

'맞습니다.'

### 4.2 if-else문
* ~가 맞다면 문장 1을 수행하고, 아니라면 문장2를 수행해라.

{% highlight html %}
if 조건문: 
    수행할 문장1
else:
    수행할 문장2
{% endhighlight %}

{% highlight html %}
likelion = 6 
if likelion == 7:
    print("멋쟁이사자처럼 7기입니다") 
else: 
    print("멋쟁이사자처럼 6기입니다") 
{% endhighlight %}

$ python practice.py

'멋쟁이사자처럼 6기 입니다'

### 4.3 if-elif-else문
* 하나가 아닌 여러조건을 테스트하기를 원할 경우에 if-elif-else문으로 수행할 수 있다.

{% highlight html %}
likelion=7
if likelion==5:
    print('멋쟁이사자처럼 5기입니다.')
elif likelion==6:
    print('멋쟁이사자처럼 6기입니다.')
else:
    print('멋쟁이사자처럼 7기입니다.')
{% endhighlight %}

$ python practice.py

'멋쟁이사자처럼 7기입니다'
* 중간에 더 여러조건을 검사하고 싶다면, elif문을 여러 개 넣으면 된다.

## 5. 반복문

### 5.1 while문

* 반복해서 문장을 수행해야 할 경우 while문을 사용한다. 그래서 while문을 반복문이라고도 부른다.
* while문은 조건문이 참인 동안에 while문 아래에 속하는 문장들이 반복해서 수행된다.

{% highlight html %}
while <조건문>:
    <수행할 문장1>
    <수행할 문장2>
    <수행할 문장3> ...
{% endhighlight %}

{% highlight html %}
likelion=1
while likelion<7:
    print("멋쟁이 사자처럼 %d기 입니다" %likelion)
    likelion= likelion+1
    if likelion == 7:
        print("우리는 행복한 멋쟁이사자처럼 7기!")
{% endhighlight %}

$ python practice.py
멋쟁이 사자처럼 1기 입니다<br>
멋쟁이 사자처럼 2기 입니다<br>
멋쟁이 사자처럼 3기 입니다<br>
멋쟁이 사자처럼 4기 입니다<br>
멋쟁이 사자처럼 5기 입니다<br>
멋쟁이 사자처럼 6기 입니다<br>
우리는 행복한 멋쟁이사자처럼 7기!

### 5.2 for문
* While문과 비슷하면서, 반복문의 역할을 하는 것에는 for문 도 있습니다.

{% highlight html %}
for 변수 in 리스트(또는 튜플, 문자열): 
    수행할 문장1 
    수행할 문장2 
    …
{% endhighlight %}

{% highlight html %}
test_list = ['one', 'two', 'three']
for i in test_list: 
      print(i) 
{% endhighlight %}

$ python practice.py
<br>one
<br>two 
<br>three

* for문 안에서도 while과 마찬가지로, if else문을 사용할 수 있습니다.
{% highlight html %}
lion_we_know=['hyojin', 'seol', 'enjin','kijung']
lion_meet = ['hyojin', 'enjin', 'bak']
	
for lion in lion_meet:
  if lion in lion_we_know:
    print("hello!")
  else:
    print("I don't know")
{% endhighlight %}

$ python practice.py
<br>hello!
<br>hello!
<br>I don't know


# <Bootstrap사용하기>

>**부트스트랩이란?**


* CSS/JavaScript기반의 웹프레임워크
* 부트스트랩은 오픈소스로 만들어졌기 때문에 무료로 사용가능
* 반응형 웹지원(자동화면조정)
* 브라우저별로 호환이 된다.

>visual studio code에 폴더를 하나 만들고, pratice.html파일을 만듭니다.


## 1. 구글에 bootstrap검색
<br><https://getbootstrap.com/>

## 2. 부트스트랩 CDN사용하기
부트스트랩 페이지 하단에 나와있는 **CDN**을 긁어서, **html문서의 head태그안**에 넣어준다.

{% highlight html %}
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/
bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0i
XCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T"  
crossorigin="anonymous">
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" 
integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4Y
fRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/
1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJ
ty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin=
"anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1
/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoII
y6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous">
</script>
{% endhighlight %}

## 3. 부트스트랩 코드 활용하기

### 3.1 navbar
Documentation - Components - navbar

body태그 안에 다음 코드를 넣어줍니다.<br>

{% highlight html %}
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="#">Navbar</a>
  <button class="navbar-toggler" type="button" 
  data-toggle="collapse" data-target="#navbarSupportedContent"
  aria-controls="navbarSupportedContent" aria-expanded="false"
  aria-label="Toggle navigation">
  <span class="navbar-toggler-icon"></span>
  </button>
<div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item active">
        <a class="nav-link" href="#">Home <span class="sr-only">
	(current)</span></a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Link</a>
      </li>
      <li class="nav-item dropdown">
        <a class="nav-link dropdown-toggle" href="#" 
	id="navbarDropdown" role="button" data-toggle="dropdown" 
	aria-haspopup="true" aria-expanded="false">
          Dropdown
        </a>
    <div class="dropdown-menu" aria-labelledby="navbarDropdown">
	<a class="dropdown-item" href="#">Action</a>
	<a class="dropdown-item" href="#">Another action</a>
	<div class="dropdown-divider"></div>
	<a class="dropdown-item" href="#">Something else here</a>
    </div>
      </li>
      <li class="nav-item">
        <a class="nav-link disabled" href="#" tabindex="-1" 
	   aria-disabled="true">Disabled</a>
      </li>
    </ul>
    <form class="form-inline my-2 my-lg-0">
      <input class="form-control mr-sm-2" type="search" 
	     placeholder="Search" aria-label="Search">
      <button class="btn btn-outline-success my-2 my-sm-0" 
	     type="submit">Search</button>
    </form>
</div>
</nav>
{% endhighlight %}


* 오른쪽 마우스 클릭 - live with server를 클릭하면, 화면을 볼 수 있습니다.

#### navbar 색깔 바꾸기1

navbar페이지 하단의 color schemes
(https://getbootstrap.com/docs/4.3/components/navbar/#color-schemes)

>**Color schemes**<br>
Theming the navbar has never been easier thanks to the combination of theming classes and background-color utilities. Choose from .navbar-light for use with light background colors, or .navbar-dark for dark background colors. Then, customize with .bg-*utilities.


<br>따라서, 다음과 같이 고치면 내부바 색깔이 파란색으로 변경된다.

{% highlight html %}
<nav class="navbar navbar-expand-lg navbar-dark bg-primary">
{% endhighlight %}

#### navbar 색깔 바꾸기2
inline css를 써서 부트스트랩에 나와있지 않은 다른 색깔로 바꾸기
{% highlight html %}
<nav class="navbar navbar-expand-lg navbar-dark" 
style="background-color:#73A8D1;">
{% endhighlight %}
bg-primary부분을 지우고, 대신 style속성에 backgrond-color를 넣어준다.

### 3.2 jumbotron
Documentation - Components - jumbotron
사이트에 처음 들어갔을 때 보이는, 사이트에 대한 간단한 설명이나 멘트같은 것을 넣어서 크게 보여주는 꾸미기

{% highlight html %}
<div class="jumbotron">
	<h1 class="display-4">Hello, world!</h1>
	<p class="lead">This is a simple hero unit, a simple 
	jumbotron-style component for calling extra attention 
	to featured content or information.</p>
	<hr class="my-4">
	<p>It uses utility classes for typography and spacing 
	to space content out within the larger container.</p>
	<a class="btn btn-primary btn-lg" href="#" role="button">
	Learn more</a>
</div>
{% endhighlight %}

### 3.3 card
Documentation - Components - card
#### card1

{% highlight html %}
<div class="card" style="width: 18rem;">
  <div class="card-body">
    <h5 class="card-title">Card title</h5>
    <h6 class="card-subtitle mb-2 text-muted">Card subtitle</h6>
    <p class="card-text">Some quick example text to build on the 
    card title and make up the bulk of the card's content.</p>
    <a href="#" class="card-link">Card link</a>
    <a href="#" class="card-link">Another link</a>
  </div>
</div>
{% endhighlight %}

card-link부분에 링크를 걸 수 있습니다.
{% highlight html %}
<a href="http://www.hufs.ac.kr/" class="card-link">Card link</a>
{% endhighlight %}

한국외국어대학교 홈페이지로 이동하는 것을 확인할 수 있습니다.
* 카드의 넓이와 높이를 조정할 수 있습니다.


{% highlight html %}
<div class="card" style="width: 68rem; height: 15rem;">
{% endhighlight %}

#### card2

이미지를 넣을 수 있는 카드 부트스트랩
{% highlight html %}

<div class="card" style="width: 18rem;">
  <img src="..." class="card-img-top" alt="...">
  <div class="card-body">
   <p class="card-text">Some quick example text to build on the
   card title and make up the bulk of the card's content.</p>
  </div>
</div>
{% endhighlight %}

이미지를 보여주기 위해, 이 html이 있는 폴더 안에 원하는 사진을 넣고, img src=" "부분에 해당 사진의 이름을 넣어줍니다.
{% highlight html %}
<img src="image1.jpg" class="card-img-top" alt="my melody">
{% endhighlight %}

이 카드를 3개를 나란히 하기 위해서, grid를 사용하겠습니다.
gird를 검색하면, 

{% highlight html %}
<div class="container">
  <div class="row">
    <div class="col-sm">
      One of three columns
    </div>
    <div class="col-sm">
      One of three columns
    </div>
    <div class="col-sm">
      One of three columns
    </div>
  </div>
</div>
{% endhighlight %}

세개의 구간을 똑같은 간격으로 나눠주는 grid코드가 있습니다.
One of three colums부분에
이미지 카드를 넣어주면, 세개의 이미지 카드가 나란히 나타날 수 있습니다.

>부트스트랩 grid

* 한행을 12개 컬럼 기준으로 크기를 지정
* "row" 클래스 div(행) 내부에 "col-종류-크기" div(컬럼) 사용

{% highlight html %}
<div class="row">
  <div class="col-sm">
    <div class="card" style="width: 18rem;">
        <img src="image1.jpg" class="card-img-top" alt="...">
        <div class="card-body">
        <p class="card-text">Some quick example text to build on 
	the card title and make up the bulk of the card's content.
	</p>
        </div>
        </div>
        </div>
        <div class="col-sm">
        <div class="card" style="width: 18rem;">
        <img src="image1.jpg" class="card-img-top" alt="...">
        <div class="card-body">
        <p class="card-text">
	Some quick example text to build on the card 
	title and make up the bulk of the card's content.</p>
        </div>
        </div>
        </div>
        <div class="col-sm">
        <div class="card" style="width: 18rem;">
        <img src="image1.jpg" class="card-img-top" alt="...">
        <div class="card-body">
        <p class="card-text">Some quick example text to build on
	the card title and make up the bulk of the card's content.
	</p>
        </div>
      </div>
   </div>
</div>
{% endhighlight %}

### 3.4 form
Documentation - Components - form

이메일과 패스워드를 입력하는 폼

{% highlight html %}
<form>
  <div class="form-group">
  <label for="exampleInputEmail1">Email address</label>
  <input type="email" class="form-control" id="exampleInputEmail1" 
	 aria-describedby="emailHelp" placeholder="Enter email">
  <small id="emailHelp" class="form-text text-muted">We'll never 
	 share your email with anyone else.</small>
  </div>
  <div class="form-group">
    <label for="exampleInputPassword1">Password</label>
    <input type="password" class="form-control" 
	id="exampleInputPassword1" placeholder="Password">
  </div>
  <div class="form-group form-check">
    <input type="checkbox" class="form-check-input" id="example
    Check1">
    <label class="form-check-label" for="exampleCheck1">
    Check me out</label>
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
{% endhighlight %}

### 3.5 button
Documentation - Components - button
{% highlight html %}
<button type="button" class="btn btn-warning">Warning</button>
{% endhighlight %}

여러종류의 버튼 중 선택하여, 폼의 submit버튼 코드 밑에 추가

### 3.6 Alerts
Documentation - Components - Alerts
알림창 부트스트랩, 각 단어 별로 색깔이 다름.

{% highlight html %}
<div class="alert alert-danger" role="alert">
  A simple danger alert—check it out!
</div>
{% endhighlight %}

	
### 3.7 container
bootstrap페이지에서 container검색(https://getbootstrap.com/docs/4.3/layout/overview/#containers)
{% highlight html %}
<div class="container">
...
<div>
{% endhighlight %}

이 container를 전체 코드에 씌워주면, 양 옆에 여백을 주며 가운데로 정렬을 시켜준다.
<br>

> bootstrap - example에 들어가면, 이미 만들어 놓은 예시들이 있습니다.

# <github 페이지에 올리기>
### 1. 레포지토리생성(github.io)
  1.1 자신의 github에 로그인 후 repository 생성 클릭
  1.2 레포지토리 이름을 **깃헙닉네임.github.io**로 해야 한다.

### 2. git 명령어

레포지토리를 생성하면 나오는 페이지에 다음과 같은 git명령어를 차례대로 터미널에 입력.

**파일을 올리기전에, 파일명은 무조건 index.html로 수정!**
{% highlight html %}
$ git init
$ git add .
$ git commit -m "message"
$ git remote add origin "<깃헙주소>"
$ git push -u origin master
{% endhighlight %}


git 명령어에 대한 설명은 세션 ppt참고!

### 3. git push가 완료 되면, 자신의 레포지토리로 돌아가, 생성했던 레포지토리이름(깃헙닉네임.github.io)를 url에 입력해본다.
만들었던 화면과 같으면 성공!

# PPT참고

[세션3.pdf](https://github.com/hyojin17/session3_folder/files/2951548/3.pdf)
