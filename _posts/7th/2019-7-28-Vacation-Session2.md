---
layout: post
title: Vacation Session 2. Python HTML Calendar를 이용한 Simple Scheduler
category: 7th
---

Vacation Session 2 - prod. 이태훈

## Session Preview
    
-   본 세션 자료에서는 Python 내장 HTML Calendar를 이용하여 간단한 Scheduler를 만들어 보도록 하겠습니다.
        

## HTML Calendar에 관하여

-   기존 Python에 내장되어 있는 Calendar 모듈의 HTML 확장형 모듈입니다. 일, 월, 연을 기준으로 간단한 달력을 작성 가능하여 작성된 달력은 HTML Table Tag로 표현됩니다. 본 세션에선 해당 HTML Calendar를 상속받은 Class를 통해 달력 내부에 Event 객체를 등록하여 HTML 문서 내에 표현하는 것을 목표로 합니다.
    

## 기본 프로젝트 세팅

-   기본적인 프로젝트 세팅은 이전과 동일한 방식으로 합니다. 세션 자료에선 HTMLCalendar라는 프로젝트에 events라는 App을 등록하는 것으로 하겠습니다.
    

{% highlight html %}
{% raw %}
$ django-admin startproject HTMLCalendar
... (cd 생략)
$ python manage.py startapp events
{% endraw %}
{% endhighlight %}

-   settings.py에 events App을 등록한 후 events 앱에 먼저 templates 폴더를 생성한 후 다음과 같은 세 파일을 만들어줍니다.

{% highlight html %}
{% raw %}
base.html
events.html
input.html
{% endraw %}
{% endhighlight %}

-   각각 템플릿 규격을 상속할 베이스 템플릿, 인덱스 페이지 역할과 동시에 등록된 이벤트들을 열람할 수 있는 달력 페이지, 이벤트 등록용 템플릿 페이지입니다.
    
-   base.html에 다음과 같이 작성해주세요.
    
{% highlight html %}
{% raw %}
{% load static %}
<html>
    <head>
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
        <link href="{% static 'css/style.css' %}" rel="stylesheet" type="text/css" />
    </head>
    <header>
        <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <a class="navbar-brand" href="{% url 'calendar' %}">나만의 달력</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'calendar' %}">홈</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'new' %}">이벤트 입력</a>
                </li>
            </ul>
        </div>
        </nav>
    </header>

    <body>
        {% block content%}
        {% endblock %}
        <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
        <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
    </body>
</html>
{% endraw %}
{% endhighlight %}

-   부트스트랩을 통한 간단한 Navbar 작성과 Container 규격을 위해 상속을 사용합니다.
-   events.html에 다음과 같이 작성해주세요.

{% highlight html %}
{% raw %}
{% extends 'base.html' %}
{% block content %}
    <div class="container">
        <br>
        <h1>나만의 달력</h1>
        <div class="outer">
            <a class="btn btn-info left" href="{% url 'calendar' %}?{{ prev_month }}"><span>이전 달</span> </a>
            <a class="btn btn-info right" href="{% url 'calendar' %}?{{ next_month }}"><span>다음 달</span> </a>
        </div>
        {{calendar}}
    </div>
{% endblock %}
{% endraw %}
{% endhighlight %}

* 마지막으로 input.html에 다음과 같이 작성해주세요.

{% highlight html %}
{% raw %}
{% extends 'base.html' %}
{% block content %}
<div class="container">
    <br>
    <form method="POST">
    {% csrf_token %}
    <div class="form form-table">
        {{ form }}
        <button type="submit" class="btn btn-info right"> Submit </button>
    </div>
    </form>
</div>
{% endblock %}
{% endraw %}
{% endhighlight %}

* 이제 URL을 설정해줍니다. 해당 세션에서 사용할 App은 하나이기 때문에 Project URL에 바로 다음과 같이 작성합니다(urls.py).

{% highlight html %}
{% raw %}
from django.contrib import admin
from django.urls import path
import events.views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', events.views.calendar_view, name="calendar"),
    path('event/new/', events.views.event, name="new"),
    path('event/edit/<int:event_id>', events.views.event, name="edit"),
]
{% endraw %}
{% endhighlight %}

* 이제 마지막으로 CSS 세팅을 한 뒤 본격적으로 Calendar를 제작하도록 합니다. events App 내에 static 폴더를 만든 뒤 하위 폴더로 css 폴더를 만들고 해당 폴더에 style.css 파일을 생성합니다. 다음과 같이 코드를 작성합니다.

{% highlight html %}
{% raw %}
@font-face { font-family: 'BMHANNAPro'; src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_seven@1.0/BMHANNAPro.woff') format('woff'); font-weight: normal; font-style: normal; }

h1 {
    text-align : center;
    font-family: 'BMHANNAPro';
}

.calendar{
    width: 70vw;
    font-size: 1rem;
    margin : auto;
	background: #fff;
	border-radius: 0 0 1em 1em;
	-webkit-box-shadow: 1px 1px 1px 1px rgba(0, 0, 0, .2), 10px 10px 10px 10px #fff;
	box-shadow: 1px 1px 1px 1px rgba(0, 0, 0, .2), 10px 10px 10px 10px #fff;
	color: #555;
    text-align : center;
}

.month {
    font-size: 1rem;
}
  
.date {
    font-size: 1rem;
}

.calendar .month {
    color : #222222;
    font-weight: 800;
}

.calendar th {
    padding: 2vw;
    text-align: center;
    font-size: 1.5rem;
}

.calendar tr {
	color: #e66b6b;
	font-weight: 700;
	text-transform: uppercase;
}

.calendar td {
    width: 1rem;
    height: 4.5em;
    padding: .25em .25em;
    transition: background .5s;
  }
  

.calendar td:hover {
	background: #cacaca;
	color: #fff;
}

.event_line {
    height: 100%;
    padding: 0;
    list-style: none;
    margin : 0;
}
  
a {
    transition: color .2s;
}

span {
    color : black;
}

a:link { color: blue; text-decoration: none;}
a:visited { color: blue; text-decoration: none;}
a:hover { color: red; text-decoration: none;}

.left {
    float : left;
    margin : 10px;
}
    
.right {
    float : right;
    margin : 10px;
}

.form {
    margin: auto;
}
  
  .form input, .form select, .form textarea {
    border-radius: 5px;
    border: 1px solid #17a2b8;
    outline: none;
    background: none;
    padding: 5px;
    width: 100%;
}
{% endraw %}
{% endhighlight %}

## Calendar 작성

* 본격적으로 Scheduler를 만들기 위해 두가지 추가 파일이 필요합니다. 먼저 HTML Calendar의 속성을 이용하여 Table Tag로 달력을 띄워주는 calendar.py 파일을 events App 내부에 새로 만들어줍니다.

* 다음과 같이 calendar.py 파일에 코드를 작성합니다.

{% highlight html %}
{% raw %}
from datetime import datetime, timedelta
from calendar import HTMLCalendar
from .models import Event

class Calendar(HTMLCalendar):
	def __init__(self, year=None, month=None):
		self.year = year
		self.month = month
		super(Calendar, self).__init__()

	# '일'을 td 태그로 변환하고 이벤트를 '일'순으로 필터
	def formatday(self, day, events):
		events_per_day = events.filter(start_time__day=day)
		d = ''
		for event in events_per_day:
			d += f'<li> {event.get_html_url} </li>'

		if day != 0:
			return f"<td><span class='date'>{day}</span><ul class='event_line'> {d} </ul></td>"
		return '<td></td>'

	# '주'를 tr 태그로 변환
	def formatweek(self, theweek, events):
		week = ''
		for d, weekday in theweek:
			week += self.formatday(d, events)
		return f'<tr> {week} </tr>'

	# '월'을 테이블 태그로 변환
	# 각 '월'과 '연'으로 이벤트 필터
	def formatmonth(self, withyear=True):
		events = Event.objects.filter(start_time__year=self.year, start_time__month=self.month)

		cal = f'<table class="calendar">\n'
		cal += f'{self.formatmonthname(self.year, self.month, withyear=withyear)}\n'
		cal += f'{self.formatweekheader()}\n'
		for week in self.monthdays2calendar(self.year, self.month):
			cal += f'{self.formatweek(week, events)}\n'
		return cal
{% endraw %}
{% endhighlight %}

* calendar.py 파일의 새로운 Calendar Class는 HTML Calendar 모듈을 상속받아 formatday, formatweek, formatmonth 세가지 함수를 Overriding(덮어쓰기) 하는 역할을 합니다.

* 논리 흐름은 formatday -> formatweek -> formatmonth의 순서와 같습니다. 먼저 '일' 칸을 만들고 '일' 칸들을 모아 '주' 행을 만든 뒤 '주' 행들을 모아 '월' 달력을 작성한다는 논리입니다. 

* 이제 Model을 작성해봅시다. events App의 models.py에 다음과 같이 새로운 모델을 작성해줍니다.

{% highlight html %}
{% raw %}
from django.db import models
from django.urls import reverse

class Event(models.Model):
    start_time = models.DateTimeField("시작시간")
    end_time = models.DateTimeField("마감시간")
    title = models.CharField("이벤트 이름", max_length=50)
    description = models.TextField("상세")

    class Meta:
        verbose_name = "이벤트 데이터"
        verbose_name_plural = "이벤트 데이터"

    def __str__(self):
        return self.title

    @property
    def get_html_url(self):
        url = reverse('edit', args=(self.id,))
        return f'<a href="{url}"> {self.title} </a>'
{% endraw %}
{% endhighlight %}

* 메타 클래스의 verbose_name은 admin 상에서 해당 객체에 대한 단수 복수 형태를 어떻게 표시할 것인지에 대한 정의입니다.

* get_html_url은 해당 객체의 공통 속성으로 적용되는 함수입니다. 해당 객체를 URL화 하였을 때 어떤 URL을 반환할 것인지 정의해줍니다.

* 이제 forms.py 파일을 events App 폴더에 새로 생성한 뒤 다음과 같이 작성해줍니다.

{% highlight html %}
{% raw %}
from django.forms import ModelForm, DateInput
from .models import Event

class EventForm(ModelForm):
    class Meta:
        model = Event
        widgets = {
            'start_time': DateInput(attrs={'type': 'date'}, format='%Y-%m-%d'),
            'end_time': DateInput(attrs={'type': 'date'}, format='%Y-%m-%d'),
        }
        fields = '__all__'
    
    def __init__(self, *args, **kwargs):
        super(EventForm, self).__init__(*args, **kwargs)
        self.fields['start_time'].input_formats = ('%Y-%m-%d',)
        self.fields['end_time'].input_formats = ('%Y-%m-%d',)
{% endraw %}
{% endhighlight %}

* 메타 클래스의 widgets 속성은 해당 Form에 어떤 종류의 Input을 넣을 것인지 특정해주는 역할을 합니다. format으로 올바른 input 형태를 미리 규정할 수 있습니다. 이후 validation 단계에서 입력이 규정되어 있는 형태와 다를 경우 오류처리를 해줄 수 있습니다.

* 이제 events App의 views.py 파일에 다음과 같이 작성해줍니다.

{% highlight html %}
{% raw %}
from django.shortcuts import render, redirect, get_object_or_404, reverse
import datetime
from .models import Event
import calendar
from .calendar import Calendar
from django.utils.safestring import mark_safe
from .forms import EventForm

def calendar_view(request):
    today = get_date(request.GET.get('month'))

    prev_month_var = prev_month(today)
    next_month_var = next_month(today)

    cal = Calendar(today.year, today.month)
    html_cal = cal.formatmonth(withyear=True)
    result_cal = mark_safe(html_cal)

    context = {'calendar' : result_cal, 'prev_month' : prev_month_var, 'next_month' : next_month_var}

    return render(request, 'events.html', context)

#현재 달력을 보고 있는 시점의 시간을 반환
def get_date(req_day):
    if req_day:
        year, month = (int(x) for x in req_day.split('-'))
        return datetime.date(year, month, day=1)
    return datetime.datetime.today()

#현재 달력의 이전 달 URL 반환
def prev_month(day):
    first = day.replace(day=1)
    prev_month = first - datetime.timedelta(days=1)
    month = 'month=' + str(prev_month.year) + '-' + str(prev_month.month)
    return month

#현재 달력의 다음 달 URL 반환
def next_month(day):
    days_in_month = calendar.monthrange(day.year, day.month)[1]
    last = day.replace(day=days_in_month)
    next_month = last + datetime.timedelta(days=1)
    month = 'month=' + str(next_month.year) + '-' + str(next_month.month)
    return month

#새로운 Event의 등록 혹은 수정
def event(request, event_id=None):
    if event_id:
        instance = get_object_or_404(Event, pk=event_id)
    else:
        instance = Event()
    
    form = EventForm(request.POST or None, instance=instance)
    if request.POST and form.is_valid():
        form.save()
        return redirect('calendar')
    return render(request, 'input.html', {'form': form})

{% endraw %}
{% endhighlight %}

* get_date는 페이지 첫 열람 시점에서의 '현재 월' 혹은 prev_month / next_month로 달력의 월을 이동하였을 시점에서의 '월'을 반환해주는 함수입니다.

* prev_month 와 next_month 함수는 모두 현재 달력 이용자가 열람하고 있는 '월' 시점에서의 이전 달과 다음 달을 계산하여 반환하는 함수입니다.

* event 함수는 새로운 Event 모델의 객체 등록 혹은 수정을 위한 함수입니다. 이를 위해 forms.py의 Custom Form인 EventForm Class를 사용합니다.

* 정상적으로 Calendar가 작성되었다면 Runserver을 통해 다음과 같이 달력을 열람할 수 있습니다.

![image](https://user-images.githubusercontent.com/46686577/62002988-2b14f200-b14a-11e9-9b86-29d4000da9b8.png)

* 이벤트 입력 시 Form의 형태는 다음과 같습니다.

![image](https://user-images.githubusercontent.com/46686577/62002991-35cf8700-b14a-11e9-884e-26f838082c5b.png)

* 해당 Scheduler는 URL 상에 열람하는 시점의 '월'이 표시된다는 단점이 있습니다. 혹시 사용하기 편한 제 3자 Django Module을 발견하셨다면 그것을 사용하셔도 좋을 것 같습니다.

* 감사합니다.
