---
layout: post
title: 1. 파이썬 문법, MTV 패턴, 부트스트랩 템플릿
category: session
---
세션 4 - prod. 김은진
# 4th session

## Contents

1. Python 문법(2) 
    1. def - 함수
    2. module
    3. class
2. "Hello World!" using MTV pattern
    1. MTV패턴이란?
    2. "Hello World!" 출력하기
3. Customizing for Bootstrap templates
    1. Bootstrap templates 이용법
    2. Customizing 방법
    3. Font Awesome의 icons 이용법

---
***
___


## 1. Python 문법(2)

### 복습
list, for, while, dictionary, tuple 등등

### 1. def - 함수
* 함수란? function : 코드의 묶임에 이름을 붙인 것으로 입력을 받아서 출력을 내보낸다.
* 함수의 정의(선언, 생성) : def 함수명(parameter):
{% highlight html %}
def print_hello():
    print("hello")
{% endhighlight %}

* 함수의 호출 
{% highlight html %}
print_hello()
{% endhighlight %}

* 함수를 호출할 때 함수가 명령을 처리하기 위해 필요한 정보를 전달할 수 있는데 이 때, 전달되는 parameters들을 인수(argument)라고 부른다.
* 함수의 수행 결과를 전달받고 싶은 경우 return문 사용한다.*
* 여러 값들을 입력에 전달하고 출력으로 받을 수 있다.

### 2. module
* module이란? 함수, 변수, 클래스들을 모아놓은 파일을 일컫으며, 다른 python 프로그램에서 불러와서 사용할 수 있도록 만들어진 .py 파일이다.
* import를 이용해 module을 불러와 사용할 수 있다.
* 함수뿐만 아니라 변수들도 저장해 불러와 사용 가능하다.

{% highlight html %}

def print_hello():
    print("hello")
{% endhighlight %}
이것을 hello.py라고 저장 후,

{% highlight html %}
import hello
hello.print_hello()
{% endhighlight %}
라고 사용가능하다.
* module.function으로 module 내의 함수를 불러와 이용 가능하다.

### 3. class
* 객체 지향 프로그래밍 (object-oriented programming)
    * 객체(object)
    : 하나의 단위로 묶인 변수(속성, attribute)와 함수들(동작, action)
    * 클래스(class)
    : 객체를 만들기 위한 일종의 틀로 템플릿(template)이라고도 한다.
    * 인스턴스(instance)
    : 클래스(틀)을 이용해 실제로 만들어진 결과물
* 클래스와 인스턴스의 생성

{% highlight html %}

class Car:
    color =""
    speed = 0

    def speedUp(self, value):
        self.speed += value
    def speedDown(self, value):
        self.speed -= value

myCar1 = Car()
myCar2 = Car()
myCar1.speedUp(30)
print(myCar1.speed)
{% endhighlight %}
* 이 외에도 __init__을 이용한 생성자, 정적 메소드 등이 사용된다.
![class-detail](https://user-images.githubusercontent.com/37901314/54296382-2c5cdc80-45f8-11e9-92d4-e4bf4f414508.PNG)


---
***
___


## 2. "Hello World!" using MTV pattern

### 1. MTV패턴이란?
* : Model, Templates, View를 일컫는 말로 3개의 파일들의 상호교환을 통해 웹이 실행된다.
![mtv](https://user-images.githubusercontent.com/37901314/54296869-0257ea00-45f9-11e9-9079-b4ffde7d0909.PNG)

1. project와 app을 생성한다.
2. settings.py에 app의 생성을 알린다.
3. templates(html)을 생성한다.
4. templates에서 사용될 객체(object)들의 속성(attribute)과 동작(action)을 model.py와 view.py에서 만들어준다.
![mtvd](https://user-images.githubusercontent.com/37901314/54296872-02f08080-45f9-11e9-959d-c8e49cd5e5a2.PNG)
![mtvdd](https://user-images.githubusercontent.com/37901314/54296874-02f08080-45f9-11e9-8482-bc7ce6a8b48a.PNG)

### 2. "Hello World!" 출력하기
* home.html에 print("Hello World!")를 입력해준다.
* view.py에 request가 들어올 시, home.html을 반환해주는 함수를 작성해준다.
* urls.py에 views.py와 home.html을 연결해주는 설정을 해준다.

* 자세한 사항(순서)은 ppt참조

[ppt](https://github.com/eunjin97/test/files/2962610/0328.pptx) 

---
***
___
## 3. Customizing for Bootstrap templates

### 1. Bootstrap templates 이용법
1. https://startbootstrap.com/를 들어가서 원하는 template을 고른 후 다운로드한다.
2. 내용물 전체를 복사해서 만들어둔 app에 static이라는 폴더를 만들고 그 안에 붙여 넣는다.
3. 내용물 중 index.html를 잘라내서 App의 templates폴더에 app의 이름으로 폴더를 하나 더 만들고, 그 안에 이 index.html을 넣는다.
4. css, javascript의 경로를 연결해준다. 
    1. index.html로 가서 최상단에 {% load staticfiles %}를 적어준다.
    * staticfile(개발자가 넣어둔 img들)를 불러와서 사용하겠다라고 알려주는 의미다.
    2. <link href='{% static "css/agency.min.css" %}' rel="styleshee"> 이런 형태로 href(하이퍼링크), 즉 경로를 다시 설정해준다.
    3. src='{% static “css/agency.min.css" %}'
 이처럼 href가 아닌 src(source, 소스) 또한 수정해주자.
 5. 서버를 돌려보고 제대로 나오는지 확인한다.

### 2. Customizing 방법
1. 크롬의 개발자도구(f12)를 이용해 원하는 소스의 위치와 정보, 속성들을 확인한다.
2. index.html로 돌아가 Cntl+F를 이용해 그 곳의 위치를 알아내서 색, 크기, 이미지 등을 원하는대로 수정해준다.

### 3. Font Awesome의 icons 이용법
1. 홈페이지에서 CDN을 긁어와 head에 붙인다.
2. 원하는 아이콘의 html를 복사한다.
3. 원하는 위치에 붙여넣는다.
    
* 자세한 사항(순서)은 ppt참조

[ppt](https://github.com/eunjin97/test/files/2962610/0328.pptx) 
