---
layout: post
title: 10. UD복습, form, JS팝업
category: session
---
세션 10 - prod. 김은진


# 10th session 

## Contents
1. UD 복습
  - update
  - delete
2. 폼(form) 
3. JS팝업

---
***
___

## 1. UD 복습

###복습

1. 환경설정(프로젝트, 앱, 모델, admin 계정 생성)

python -m venv myvenv<br>
. myvenv/Scripts/activate<br>
pip inistall django<br>
django-admin startproject form<br>
cd form<br>
python manage.py startapp blogapp<br>

#settings.py<br>
blogapp.apps.BlogappConfig


{% highlight html %}
{% raw %}
#models.py
class Blog(models.Model):
     title = models.CharField(max_length=100)
     pub_date = models.DateTimeField('date published')
     body = models.TextField()
     def __str__(self):
              return self.title    
{% endraw %}
{% endhighlight %}


python manage.py makemigrations<br>
python manage.py migrate

#admin.py<br>
from .models import Blog<br>
admin.site.register(Blog)<br>

terminal<br>
python manage.py createsuperuser<br>
python manage.py runserver<br>

#views.py<br>
{% highlight html %}
{% raw %}
from .models import Blog
def home(request):
      blogs=Blog.objects
      return render(request, 'home.html',{'blogs':blogs})
{% endraw %}
{% endhighlight %}

#home.html<br>
{% highlight html %}
{% raw %}
<h1>This is Home page.</h1>
{% for blog in blogs.all %}
<div class="container">
    <h1>{{blog.title}}</h1>
    <p>{{blog.pub_date}}</p>
    <p>{{blog.body}}</p>
</div>
{% endraw %}
{% endhighlight %}

-------------------------------------------------------------
2. detail page
{% highlight html %}
{% raw %}
#views.py
get_object_or_404
def detail(request, blog_id):
    details = get_object_or_404(Blog, pk=blog_id)
    return render(request, 'detail.html',{'details':details})

#detail.html
<h1>This is detail page.</h1>
<h2>{{details.title}}</h2>
<p>{{details.pub_date}}</p>
<p>{{details.body}}</p>
<a href="{% url 'home' %}">home으로</a>

#urls.py
import blogapp.views
    path('',blogapp.views.home, name="home"),
    path('blog/<int:blog_id>/',blogapp.views.detail,name='detail'),

#home.html
<a href="{% url 'detail' blog.id %}">
{% endraw %}
{% endhighlight %}
  
-------------------------------------------------------------
3. new, create
{% highlight html %}
{% raw %}
#views.py
from django.utils import timezone
def new(request):
        return render(request, 'new.html)
def create(request):
      blog = Blog()
      blog.title = request.POST[title]
      blog.pub_date = timezone.datetime.now()
      blog.body = request.POST['body']
      blog.save()
      return redirect('/blog/'+str(blog.id))

#home.html
<a href="{% url 'new' %}">new post </a>
#new.html
<div class="container">
    <form action="{% url 'create' %}" method="POST">
    {% csrf_token%} 
    <h4>제목: </h4>
    <input type="text" name="title">
    <br>
    <h4>본문: </h4>
    <textarea cols=40 rows=10 name="body"></textarea>
    <br>
    <input class="btn btn-dark" type="submit" value="제출하기">
</form>
</div>

#urls.py
 path('blog/new/',blogapp.views.new,name='new'),
 path('blog/create/',blogapp.views.create, name='create'),
 
{% endraw %}
{% endhighlight %}

---------------------------------------------------------

4. edit, update 
{% highlight html %}
{% raw %}
#views.py
def delete(request, blog_id):
    destroy = get_object_or_404(Blog,pk=blog_id)
    destroy.delete()
    return redirect('home')
def update(request, blog_id):
     updates=get_object_or_404(Blog, pk=blog_id)
     return render(request, 'update.html',{'updates':updates})
def edit(request, blog_id):
    edit = Blog.objects.get(pk=blog_id)
    edit.title = request.POST['title']
    edit.pub_date = timezone.datetime.now()
    edit.body = request.POST['body']
    edit.save()
    return redirect('home')

#urls.py
path('blog/delete/<int:blog_id>/', blogapp.views.delete, name='delete'),
path('blog/update/<int:blog_id>/', blogapp.views.update, name='update'),
path('blog/edit/<int:blog_id>/', blogapp.views.edit, name='edit'),

#home.html
    <a href = "{% url 'delete' blog.id %}"> delete </a>
    <a href ="{% url 'update' blog.id %}">edit</a>

#detail.html
<a href="{% url 'delete' details.id %}">delete</a>
<a href ="{% url 'update' details.id %}">edit</a>
<a href="{% url 'home' %}">home으로</a>

{% endraw %}
{% endhighlight %}

* edit - update가 저장되도록 (create - new 같은 개념)
* home에서도 delete, edit 가능하도록, detail page에서도 deltet, edit 가능하도록 설정
-> 각 html에서 사용될 객체이름 확인해서 id 값 넣어주기!!

--------------------------------------------------------------


---
***
___


## 2. form 
  * Form : HTML에서 웹 페이지상에서 한 개 이상의 필드나 위젯들의 묶음을 말하며, 
  사용자로부터 정보를 수집하여 서버에 제출하는데 이용된다.
  
  * 일반 form이 아니라 model을 기반으로 한 form을 쓰는 이유:
  - 기존에는 model에서 title, content, date 등을 정의해주고 그 속성들 그대로 html을 제작한다.
  - model 기반/맞춤형 form을 통해 이전의 귀찮은 작업들을 쉽게 편리하게 이용/저장가능해진다.
  - html로 하나하나씩 만들면 하나하나 유효성 검사, 성공, 실패 등 이에 맞는 form 태그를 따로 만들어줘야 하는 한계성이 있어서
  django에서 제공해주는 form 태그를 이용해준다.
  

  * ex) form과 model form 비교:
  
  {% highlight html %}
  {% raw %}
  #In model.py

  from django import forms
  from .models import Post

  #Form (일반 폼)
  class PostForm(forms.Form):
    title = forms.CharField()
    content = forms.CharField(widget=forms.Textarea)

  #Model Form (모델 폼)
  class PostForm(forms.ModelForm):
    class Meta:
      model = Post
      fields = ['title', 'content']
  {% endraw %}
  {% endhighlight %}
  
  * 방법:
  - form은 django의 가장 큰 feature 중 하나로 model 클래스와 유사하게 form class를 정의한다.
  - forms.py 는 models.py와 유사한 방식으로 작성되며, model이 database에 적용되는 것처럼 form은 html의 <form>태그에 적용된다. 
  - 일반적인 폼(form)은 직접 필드를 정의하거나 위젯 설정이 필요한데, 
  모델폼(model form)은 모델과 필드를 지정하면 모델폼이 자동으로 폼필드를 생성한다.

  
  * model을 기반으로 한 입력공간만들기 개요:
          from django import forms.ModelForm  
          from django import forms
          class myForm(forms.ModelForm):
              class Meta:
                        어떤 모델을 기반으로 한 입력공간이니?
                         그 모델 중에서 어떤 항목을 입력받을거니?
 


    
### 실습

1. forms.py 생성

  - models.py파일과 동일한 경로에 forms.py파일 생성 
  - views.py에서 import해와서 사용할 것이다.
  - 효율적인 코드관리를 위해 파일을 나눈다.

  - model 기반한 form 생성해주기

  {% highlight html %}
  {% raw %}

  from django import forms
  from .models import Blog

  class BlogPost(forms.ModelForm):
          class Meta:
              model = Blog
              fields = ['title','body']

  {% endraw %}
  {% endhighlight %}
  
  * forms.py를 models.py 입력하던 것처럼 
  - 메타클래스를 선언하고 model, fields를 정의해준다.
  - class Meta: : 클래스를 만드는데 사용되는 것으로, 메타클래스의 인스턴스가 클래스라는 것이다.
  - model = Blog : 'Blog'라는 모델을 기반으로 form을 만들 것인가 정의
  - fields = [] : 모델의 속성 중엥서 입력받길 원하는 속성 정의


2. views.py 수정

  * 서버로 데이터를 전송하는 방식
  - POST방식 : 보이지 않는 타입으로, form에서 받은 데이터들을 url에 직접 보여주지 않고 데이터들을 저장해서 서버로 보낸다.
  - GET방식 : url에 데이터가 그대로 보이는 타입으로, 우리가 만든 form을 보여주면 된다.

  * 개요
  from .form import myForm
  def create(request):
      1. 처음 new.html에 들어갔을 때 빈 입력공간 띄우기 -> GET
      2. 이용자가 뭘 입력하면 그 입력값들 처리하기 -> POST
  -> if else문으로 작성하기


* 코드
  {% highlight html %}
  {% raw %}
  #forms.py의 BlogPost라는 class를 가져와서 사용
  from .forms import BlogPost

  def blogpost(request):
  #request의 method가 post방식인 경우, 새로운 blog를 post한다는 뜻이므로 사용자가 form에 입력한 데이터를 저장한다는 뜻
      if request.method =='POST':
          form = BlogPost(request.POST)
          if form.is_valid():
              post = form.save(commit=False)
              post.pub_date=timezone.now()
              post.save()
              return redirect('home')
  #request가 get방식인 경우, 사용자가 입력하려고 blogpost(새로만들기)을 누른 것이므로 입력받으려는 form을 보여줘라는 뜻
      else:
          form = BlogPost()
          return render(request,'new.html',{'form':form})
  {% endraw %}
  {% endhighlight %}

  * post인 경우
  - form = BlogPost(request.POST) : BlogPost 객체를 만들어 받아온 데이터를 입력한다.
  - is_valid() : 입력값들이 유효한지 판단하고 검사하는 메소드 함수로, 
  모든 입력값이 다 입력되었는지 모든 항목이 다 입력되었다면 형식에 맞게 입력되었는지를 검사하는 함수다.
  만일 모든 항목이 잘 입력되었다면 True를 반환하고 입력값에 문제가 있다면 False를 반환한다.
  - form.save(commit=False) : 일단 저장하지 않고, form 객체 말고 model 객체에 접근해 model 객체 내의 date 변수에 저장 후 model 객체 저장
  
   * Blog 객체의 모든 항목을 입력받아야하나? Date는 views.py에서 자동으로 값을 넣자!
  - 저장하지 않고 가져오는 이유는 pub_date를 입력받지 않고 자동으로 views.py함수를 통해 입력받을 것이기 때문이다.
  - 즉, pub_date 라는 속성에 timezone.now()은 현재 시각이 들어가도록 사용자가 직접 입력하지 않아도 자동으로 blog을 쓸 때 입력되도록 한다.
  - post.save() : form을 통해 입력받은 내용과 timezone.now()를 통해 입력받은 내용을 저장한다.

  * get인 경우
  - BlogPost 객체를 만들어 객체 내의 attribute들을 입력받도록 form에 넣은 후 return을 통해 보여준다.
  - 요청이 들어오면 new.html을 띄우는데 form은 dictionary 자료형으로 넣어서 전달한다.


3. urls.py 수정

  {% highlight html %}
  {% raw %}

  #blog/newblog로 접속하면 views.py의 blogpost라는 함수가 실행되도록 path 설정
  path('blog/newblog/', blogapp.views.blogpost, name='newblog')

  {% endraw %}
  {% endhighlight %}

4. templates 수정

  {% highlight html %}
  {% raw %}

<div class='container'>
    {% csrf_token %}
    <table>
        {{form.as_table}}
    </table>
    <br>
    <input class="btn btn-dark" type="submit" value="제출하기">
  </div>
  {%endblock%}


  {% endraw %}
  {% endhighlight %}

  * {{form}} : form 안의 내용을 어떤 태그로 감싼 채 출력할지 미리 결정 가능
  * as_table : form을 table형식으로 출력하는 form의 method(이외에도 as_p, as_ul 등이 있다.)
  * input 태그를 통해 submit버튼을 클릭하면 작성했던 form이 views.py로 이동되어 처리된다.
  


* ppt
<https://github.com/eunjin97/test/files/3048935/2019-5-2-10th-session.pdf>
---
***
___


## 3. JS 팝업(django 필요 없이 html만으로도 가능)

* 팝업은 회원가입이나 아이디 중복검색, 우편번호 검색등을 사용할 때 주로 사용
* popup.html에서 팝업창 호출하는 페이지(부모창), popuptest.html에서 팝업창 페이지(자식창), jquery.html은 팝업창에서 이동하는 페이지

1. 팝업 호출 글(부모창) 작성

2. input 태그 작성 

{% highlight html %}
{% raw %}

<input type="button" value="팝업창 호출" onclick="showPopup();" />

{% endraw %}
{% endhighlight %}

- 클릭할 때 무언가 튀어나와야(pop up)하는 input태그 안에 onclick을 통해
시도하고자 하는 java script의 function의 이름을 적는다.

3. script 작성

{% highlight html %}
{% raw %}

<script>
function showPopup() { window.open("popuptest.html", "popup1", "width=400, height=300, left=100, top=50"); }
</script>

{% endraw %}
{% endhighlight %}

- script는 onclick을 통해 시행되는 java script로 소괄호를 호출하는 메소드다.
- window.open : 화면을 띄우는데 팝업창으로 열릴 html을 적고,
popup1은 이 팝업창이 어떤 것을 의미하는지 알아보기 쉽게 적고,
그 외는 크기와 위치 등을 정할 수 있다.


4. popuptest.html(팝업창)
- 이 html이 팝업창이라는 것을 보여주기 위해 popup이라고 적는다.
- 정밀하게 팝업창의 크기를 조절하려면 바디태그에 body onload = "window.resizeTo(300,300)" 과 같이 조절 가능

<input type = "button" value = "닫기" onclick="self.close();" /> 
<input type = "button" value = "이동 후 닫기" onclick="moveClose();" />

- 닫기 버튼 : input 태그에 onclick= "self.close();"를 통해 self 자기자신 창을 닫는 메소드를 추가
- 이동 후 닫기 버튼 : input 태그에 onclick 속성을 넣어 이동 후 닫히는 onclick="moveClose();를 통해 이동 후 닫히는 메소드 추가
- self.close()는 이미 정의된 java script(js) 메소드고 moveClose는 우리가 만들 것이기 때문에 밑에 script를 열어 새로 정의를 한다.

{% highlight html %}
{% raw %}

<script>
    function moveClose(){
        opener.location.href="jquery.html";
        self.close();
    }
</script>

{% endraw %}
{% endhighlight %}

- opener.location.href : 원하는 해당 url로 가기 위해 사용하는 방법으로 opener는 부모창과 자식창을 가진다.
- location : 브라우저의 창의 열려있는 문서의 url을 알려주는 객체로 opner 없이는 부모페이지가 액션을 받지 못한다.
- 부모창 : 처음 버튼을 클릭하는 페이지로 자식창의 정보를 가지고 있다.
- 자식창 : 팝업창(눌러서 새로 생성되는 페이지)으로 부모창의 정보를 가지고 있다.

- script에 js를 만들 수도 있고 <head>에 넣어도 되고, js파일을 만들어 <head>에 링크를 연결해서 사용도 가능

5. result.html(이동될 페이지)
- 이동 후 닫기 버튼을 누른 뒤에 뜨는 페이지

* ppt
<https://github.com/eunjin97/test/files/3048935/2019-5-2-10th-session.pdf>
---
***
___
