---
layout: post      # 수정 NO 
title: 7. 게시판만들기(UD) # 글 제목으로 수정
category: session # 본인에게 맞는 카테고리명을 적으세요
---

세션 7 - prod. 장윤서 

# 7th session

## contents
1. 글 삭제(delete) 기능 구현하기 
2. 글 수정(update) 기능 구현하기 



### 본격적 코딩!!

자 저번까지 글을 읽고(Read) 쓰는 (Create) 까지 해보았으니 이젠 쓴 글을 삭제하는 Delete 와 쓴 글의 내용을 수정하는 Update 기능을 구현해볼거에요 
먼저 Delete 부터 구현해봅시다 !!   

views.py 
{% highlight html %}

def delete(request, blog_id):
//요청을 받을 때 blog_id 값을 넘겨 받고
    destroy = get_object_or_404(Blog, pk=blog_id)
//BLog 모델에서 앞의 blog_id 를 pk로 삼아서 그에 대한 특정 객체(하나의 게시물)를 찾고
//그 찾은 객체(하나의 게시물)을 destroy라는 변수에 담고
    destroy.delete()
//destroy에 담긴 객체(게시물)을 삭제한다
    return redirect('home')
//끝나면 바로 home이라는 url을 실행한다

{% endhighlight %}    

urls.py
{% highlight html %}

urlpatterns = [
		path('admin/', admin.site.urls),
		path('', blogapp.views.home, name="home"),
    path('blog/<int:blog_id>/', blogapp.views.detail, name='detail'),
    path('blog/new/', blogapp.views.new, name='new'),
    path('blog/create/', blogapp.views.create, name='create'),
    path('blog/delete/<int:blog_id>/', blogapp.views.delete, name='delete'),
    //blog/delete/(게시물번호)로 되어있는 url를 받으면, blogapp내의 views.py의 delete 함수를 실행하고, 이 url의 이름은 delete이다
]

{% endhighlight %}

home.html

{% highlight html %}
{%raw%}{% for blog in blogs.all %}
<div class="container">
    <a href="{% url 'detail' blog.id %}">
        <h1>{{ blog.title }}</h1>
    </a>
    <p>{{ blog.pub_date }}</p>
    <p>{{ blog.body }}</p>
</div>
<a href = "{% url 'delete' blog.id%}"> 삭제하기 </a>
// delete라는 이름의 url을 실행하고 이때 blog의 id 값을 넘겨준다
{% endfor %}
<a href="{% url 'new' %}"> new post </a>
{%endraw%}{% endhighlight %}

그러면 이제 이렇게 home 화면에 삭제하기 링크가 생기고 누르면 글이 삭제 됩니다!
![삭제하기버튼 생긴 home화면](https://user-images.githubusercontent.com/26545947/55211435-4ff66880-522f-11e9-971d-847cd6940b71.png)

그럼 이제 CRUD에서 남은 U(update)기능을 완성시켜 봅시다!
먼저 수정하는 페이지인 update 페이지를 만들어 봅시다 
평소와 같이 url / view / template(update) 를 설정해 봅시다

urls.py

{% highlight html %}{%raw%}
urlpatterns = [
	path('admin/', admin.site.urls),
	path('', blogapp.views.home, name="home"),
        path('blog/<int:blog_id>/', blogapp.views.detail, name='detail'),
        path('blog/new/', blogapp.views.new, name='new'),
        path('blog/create/', blogapp.views.create, name='create'),
        path('blog/delete/<int:blog_id>/', blogapp.views.delete, name='delete'),
        path('blog/update/<int:blog_id>/', blogapp.views.update, name='update'),
	//blog/update/게시물번호/ 의 요청이 들어오면, blogapp의 view의 update 함수를 불러온다, 이때 이 url의 이름은 update이다
]
{%endraw%}{% endhighlight %}


views.py
{% highlight html %}{%raw%}
def update(request, blog_id):
//요청과 blog_id이라는 변수 값이 같이 들어올때
    updates= get_object_or_404(Blog, pk=blog_id)
    //Blog 모델에서 pk를 blog_id 값으로 하여 특정 객체(특정한 게시물)을 찾아라 만약 못찾으면 404 error를 보내라 이때 이 찾은 값을 
    //updates라는 변수에 담는다
    return render(request, 'update.html', {'updates': updates})
    //요청에 대해서 update.html을 보여주고 위의 updates(view)라는 변수를 updates(update.html)라는 변수에 담아서 template에 보낸다
{%endraw%}{% endhighlight %}

templates 파일에 update.html이라고 파일을 만든 뒤 

update.html
{% highlight html %}{%raw%}
<h1> 수정 페이지 입니다 </h1>
<div class="container">
<form action="" method="POST" >
//제출하기 버튼을 눌렀을때 실행되는 url은 ""이고 이때 POST방식으로 실행된다
{% csrf_token %}
//보내는 POST값이 안전한 데이터이다 라고 명시해주는 token
   <h4>제목: </h4>
   //이때 view에서 updates라는 변수에 담긴 특정 게시물에 대한 내용을 제목, 내용 각각 꺼내야 하므로 밑에와 같이 
   //updates.title   updates.body라고 표현해줍니다
   <input type="text" name="title" value={{updates.title}}>
   <br>
   <br>
   <h4>본문: </h4>
   <textarea cols=40 rows=10 name="body">{{updates.body}}</textarea>
   <br>
   <br>
   <input class="btn btn-dark" type="submit" value="제출하기">
</form>
</div>
{%endraw%}{% endhighlight %}

home.html
{% highlight html %}{%raw%}
{% for blog in blogs.all %}
<div class="container">
    <a href="{% url 'detail' blog.id %}">
        <h1>{{ blog.title }}</h1>
    </a>
    <p>{{ blog.pub_date }}</p>
    <p>{{ blog.body }}</p>
</div>
<a href = "{% url 'delete' blog.id%}"> 삭제하기 </a>
<a href = "{% url 'update' blog.id%}"> 수정하기 </a>
//update라는 이름의 url을 실행하고 이때 blog의 id값을 같이 넘겨준다
{% endfor %}
<a href="{% url 'new' %}"> new post </a>
{%endraw%}{% endhighlight %}

이렇게하면 다음과 같이 수정하기가 생기고
![수정하기버튼 생긴 home화면](https://user-images.githubusercontent.com/26545947/55211436-52f15900-522f-11e9-9eee-a141edaa0c61.png)

수정하기를 누르면 다음과 같이 update 페이지에 수정하고자 하는 글의 제목과 내용이 나오는걸 볼 수 있습니다!
![update화면](https://user-images.githubusercontent.com/26545947/55211440-54bb1c80-522f-11e9-8a1f-e5a7365ccf24.png)

하지만 저 상태에서 제출하기를 눌러도 수정이 안됩니다 왜냐하면 아직 수정한 내용을 저장하는 기능은 구현하지 않았기 떄문이죠 
그럼 이제 수정한 내용을 저장이 되도록 기능을 구현해 봅시다! 

urls.py
{% highlight html %}{%raw%}
urlpatterns = [
	path('admin/', admin.site.urls),
	path('', blogapp.views.home, name="home"),
        path('blog/<int:blog_id>/', blogapp.views.detail, name='detail'),
        path('blog/new/', blogapp.views.new, name='new'),
        path('blog/create/', blogapp.views.create, name='create'),
        path('blog/delete/<int:blog_id>/', blogapp.views.delete, name='delete'),
        path('blog/update/<int:blog_id>/', blogapp.views.update, name='update'),
        path('blog/edit/<int:blog_id>/', blogapp.views.edit, name='edit'),
	//blog/edit/게시물의 번호의 url이 실행되면, blogapp의 view안의 edit 함수를 실행해라, 이때 이 url의 이름은 edit이다.
]
{%endraw%}{% endhighlight %}

views.py 
{% highlight html %}{%raw%}
def edit(request, blog_id):
//요청을 받을때 blog_id 값도 같이 받아서 
    edit = Blog.objects.get(pk=blog_id)
    //Blog 모델에서 앞의 blog_id를 pk값으로 삼아서 특정 객체(특정 게시물)을 찾아서 
    //edit 이라는 변수에 넣고
    edit.title = request.POST['title']
    //그 edit에 담긴 객체(게시물)의 title값은 요청한 POST 방식의 title 값이고
    edit.body = request.POST['body']
    //그 edit에 담긴 객체(게시물)의 body값은 요청한 POST 방식의 body 값이고
    edit.pub_date = timezone.datetime.now()
    //그 edit에 담긴 객체(게시물)의 pub.date값은 요청한 POST 방식의 timezone 값이고
    edit.save()
    //edit 객체에 대한 내용을 저장한다
    return redirect('home')
    //위의 과정이 다 끝나면 home 이라은 이름의 url을 실행한다
{%endraw%}{% endhighlight %}

update.html
{% highlight html %}{%raw%}
<form action="{% url 'edit' updates.id%}" method="POST" >
//form 양식을 제출하면 edit 이라는 이름의 url을 실행하고 이때 update의 id 값을 같이 전해주며 이 방식은 POST 방식이다
{%endraw%}{% endhighlight %}
	
이렇게 코드를 설정하고 수정하기 버튼을 다시 눌러보면 수정이 정상적으로 작동 됩니다!!
드디어 CRUD 끝! 

## 매우 간단한 Jquery 써보기 
Jquery에 들어가기에 앞써서 먼저 Jquery가 무엇인지에 대해 말해보자면 
Jquery는 매우 간단히는 자바스크립트 라이브러리(자주 사용하는 코드를 재사용할 수 있도록 해서 프로그래밍 효율을 높혀주는 코드들)입니다. 근데 갑자기  Jquery를 쓰는 이유는! 1. 엘리먼트를 선택하는 강력한 방법 2. 선택한 엘리먼트들을 효율적으로 제어 할 수 있는 수단을 제공 이 있으므로 
이런 유용한 친구인 Jquery에 대해서 간단히 알아보고자 합니다

 home.html에 다음과 같이 코드를 추가해주세요

home.html
{% highlight html %}{%raw%}
    <head>
        <meta charset="utf-8"/>
        <!-- <script tpye= "text/javascript" src="jquery.js"></script>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script> -->
        <script src="https://code.jquery.com/jquery-3.0.0.js"></script>
        <script src="https://code.jquery.com/jquery-migrate-3.0.1.js"></script>
    </head>
    <body>
        <ul id = "list">
            <a href=''>a</a>
            <li>empty</li>
            <li>empty</li>
            <li>empty</li>
            <li>empty</li>
            <li>empty</li>
        </ul>
        <input type="button" value = "execute" id = "execute_btn" />
        <script type = "text/javascript">
        //버튼을 클릭했을 때 함수를 넣어준다.
	//$('#')
	//여기서 샾은 body에 있는id태그 값을 읽어오는때에 씀 
            $('#execute_btn').click(function(){
            
                $('#list a').text('coding everybody');
            })
            // execute라는 value를 가진 button을 누르면 기능이 실행 되는데 그 기능은 
            // id 값이 list 고 그 안에 a 에 담겨있는 text를 'coding everybody' 바꿔라! 라는 뜻
        </script>
    </body>
</html>
{%endraw%}{% endhighlight %}

이제 이 페이지에 있는 버튼을 누르면 다음과 같이 새로 고침 없이 jquery덕에 기능이 활성화 되는걸 볼 수 있습니다 
![jquery버튼누르기전](https://user-images.githubusercontent.com/26545947/55211443-584ea380-522f-11e9-88d6-80899db2aa3e.png)
![jquery버튼누른후](https://user-images.githubusercontent.com/26545947/55211445-5a186700-522f-11e9-9f97-6f69d7c3da02.png)

하나 더 해봅시다 ! 방금 추가한 코드는 지우고 아래의 코드를 넣어주세요 
{% highlight html %}{%raw%}
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>click demo</title>
  <style>
  p {
    color: red;
    margin: 5px;
    cursor: pointer;
  }
  p:hover {
    background: yellow;
  }
  </style>
  <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
</head>
<body>
 
<p>First Paragraph</p>
<p>Second Paragraph</p>
<p>Yet one more Paragraph</p>
 
<script>
$( "p" ).click(function() {
//p태그를 글릭하면 기능이 실행이 되는데 
  $( this ).slideUp();
//그 클릭한 것이 slidUP된다
});
</script>
 
</body>
</html>
{%endraw%}{% endhighlight %}
 
그럼 다음과 같은 화면이 나타나게 되고 각각 글씨를 누르면 하나씩 사라지는걸 볼 수 있습니다! 
![jquery2 누르기전](https://user-images.githubusercontent.com/26545947/55211448-5be22a80-522f-11e9-916a-339e60e892d3.png)


이렇게 정말 간단한 Jquery 기능을 2개 정도 다뤄보았습니다     
오늘 세션은 여기까지! 수고하셨습니다~~

# jquery ppt
[session7.pdf](https://github.com/hufslion/for-pic-url/files/3021257/session7.pdf)
