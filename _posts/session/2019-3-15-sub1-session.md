---
layout: post      # 수정 NO 
title: 예비 세션 1. 쿠키커터 # 글 제목으로 수정
category: session # 본인에게 맞는 카테고리명을 적으세요
---

Cookiecutter
===========

세션 sub - prod. 장윤서

### cookiecutter 란?

cookiecutter는 Django 안에 있는 여러 package중에 하나로 본래 프로젝트 생성시 추가적으로 만들어야 되는 파일들이나 설정들을 한번에 처리해주는
역할을 합니다


### cookiecutter가 정확히 하는 일?

* 설정 스크립트를 통해서 설정 변수의 내용을 설정 
* 입력된 내용들을 통해서 프로젝트에 대한 추가적인 파일들 생성 
* 세팅, requirements, 초기문서, 테스트 환경 등을 지정


### 본격적 코딩!

가장 먼저 파일을 하나 생성하고 그 파일안에 들어간 다음 가상환경 파일을 만들어봅시다 

{% highlight html %}
python -m venv myvenv // 가상환경 파일 설치
{% endhighlight %}

그후 가상환경을 키고 django 설치까지 해봅시다

{% highlight html %}
source myvenv/Scripts/activate // 가상환경 키기 
pip install django // djnago 설치
{% endhighlight %}

django 설치까지 완료 됬으면 쿠키커터 패키지를 설치하고 쿠키커터로 프록젝트를 생성해봅시다
{% highlight html %}
pip install cookiecutter // 쿠키커터 설치
cookiecutter https://github.com/pydanny/cookiecutter-django 
// 쿠키커터 프로젝트 생성 
{% endhighlight %}

이렇게 명령어를 친후에는 앞에 말한 설정 스크립트가 뜰겁니다 이제 스크립트에 각자 자신이 원하는 대로 설정하면 됩니다 
(만약 아직 스크립트 내용이 너무 생소하다면 프로젝트 이름 작성 후부턴 쭉 엔터만 쳐도 됩니다) 

{% highlight html %}
project_name [My Awesome Project]: test_project // 프로젝트 이름 생성
project_slug [test_project]: // 해당 이름으로 폴더를 생성. 
default로 프로젝트의 이름과 같다.
description [Behold My Awesome Project!]: test for cookiecutter 
// 현재 프로젝트에 대한 간단한 설명
author_name [Daniel Roy Greenfeld]: jangyoonseo// 만든이
domain_name [example.com]: test_project.com // 도메인 이름
email [jo@example.com]: joe1220@daum.net // 이메일
version [0.1.0]:
Select open_source_license:  // 어떤 오픈소스 라이센스를 쓸지 선택
                                .default로 MIT가 선택되어 있다.
1 - MIT
2 - BSD
3 - GPLv3
4 - Apache Software License 2.0
5 - Not open source
Choose from 1, 2, 3, 4, 5 [1]:
timezone [UTC]: Asia/Seoul // 프로젝트의 시간대 설정. 서울로 설정
windows [n]: // windows에서 개발을 하는지 물어본다. yes로 설정해야 하나, 
                no로 해도 문제 없음
use_pycharm [n]: pycharm이란 파이썬 IDE를 사용하냐고 묻는데, 
                사용하지 않으니 default인 n로 넘어가겠다.
use_docker [n]: docker 사용여부를 묻는다.
Select postgresql_version: // 장고는 default로 postgresql이란 
                              데이터베이스를 사용한다.
                            어떤 postgresql의 버전을 사용중인지 묻는다.
1 - 10.4
2 - 10.3
3 - 10.2
4 - 10.1
5 - 9.6
6 - 9.5
7 - 9.4
8 - 9.3
Choose from 1, 2, 3, 4, 5, 6, 7, 8 [1]:
Select js_task_runner: Gulp.js 사용여부를 묻는다.
1 - None
2 - Gulp
Choose from 1, 2 [1]:
custom_bootstrap_compilation [n]:
use_compressor [n]:
use_celery [n]:
use_mailhog [n]:
use_sentry [n]:
use_whitenoise [n]:
use_heroku [n]:
use_travisci [n]:
{% endhighlight %}

스크립트 입력 완료후 프로젝트가 다 만들어 졌다면 추가로 pipenv를 설치해줍시다 

{% highlight html %}
pip install pipenv
{% endhighlight %}

pipenv는 pip와 virtualenv가 합쳐진 것으로써, 파이썬의 가상환경안에서 패키지를 관리할 수 있도록 해주는, python.org 에서 공식적으로 추천하는
패키지 관리 도구입니다

pipenv 설치 후 만들어진 프로젝트 파일 내에 들어간 후 추가로 명령어를 쳐줍시다 

{% highlight html %}
cd test_project // test_project 파일에 들어간다 
pipenv install -r requirements/local.txt // 
프로젝트의 requirements 폴더 안에 local.txt라는 파일을 통해 
local환경에 필요한 패키지들을 가상환경 안에 설치
{% endhighlight %}

그 다음 설정을 완료하기 위해서 먼저 참조된 ppt를 통해서 posrgresql 설치를 완료해주세요!
이제 우리의 프로젝트에 설치한 postgresql DB를 연결해줍시다!  
config 폴더/ settings 폴더/ base.py 에 DATABASES부분의 41번줄에 이 코드가 보일텐데 이제 저 default 부분를 설정해주면 됩니다 

{% highlight html %}
DATABASES = {
'default': env.db('DATABASE_URL', 
default='postgres:///testsqlite'),
}
{% endhighlight %}

앞의 default 부분을 다음 양식에 맞게 넣으면 됩니다

{% highlight html %}
'postgres://유저이름:비밀번호@localhost:포트번호/데이터베이스이름'
{% endhighlight %}

그후 서버를 킨후 들어가면 완성! 

{% highlight html %}
python manage.py runserver
{% endhighlight %}

## PostgreSQL 설치 ppt
<https://github.com/hufslion/for-pic-url/files/2970737/PostgreSQLinstall.pdf>
