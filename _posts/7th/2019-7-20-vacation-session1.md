---
layout: post
title: 방학세션 1. AWS배포하기
category: 7th
permalink: :categories/:title
---

방학세션1 - prod. 최효진


# summer vacation session 

## Contents
AWS배포하기(ElasticBeanstalk)

## 1. 사전준비

* 배포하려는 프로젝트폴더 준비.
* AWS사이트 로그인 해놓기


## 2. 가상환경설정

* virtualenv로 가상환경생성
{% highlight html %}
{% raw %}
$ pip3 install virtualenv
{% endraw %}
{% endhighlight %}

1) 이름이 eb-virt인 가상환경 생성

{% highlight html %}
{% raw %}
$ virtualenv eb-virt
{% endraw %}
{% endhighlight %}

2) 가상환경 활성화
{% highlight html %}
{% raw %}
$ source eb-virt/Scripts/activate
{% endraw %}
{% endhighlight %}

* (eb-virt)가 명령프롬프트 앞에 추가됨. -> 가상환경에 있음을 나타내는 것.

## 3. django설치

1) 다음 장고버전을 설치.
{% highlight html %}
{% raw %}
$ pip install django==2.1.1
{% endraw %}
{% endhighlight %}

* 이미 장고가 설치되어있을때,
{% highlight html %}
{% raw %}
$ pip uninstall django
$ pip install django==2.1.1
{% endraw %}
{% endhighlight %}

2) 장고가 설치되었는지 확인
{% highlight html %}
{% raw %}
$ pip freeze
Django==2.1.1
...
{% endraw %}
{% endhighlight %}


## 4. 환경변수 설정

1) "내문서 -> APPDATA -> Roming -> Python -> python37 -> Scripts"
위치의 주소를 복사.<br>
ex) C:\Users\samsung\AppData\Roaming\Python\Python37\Scripts

2) 내PC -> 오른쪽 마우스 클릭 -> 속성 -> 고급시스템설정 -> 환경변수
3) PATH클릭 -> 편집 클릭 -> 새로만들기 -> 주소복사한것 입력

## 5. awsebcli설치하기

1) ebcli설치 
{% highlight html %}
{% raw %}
$ pip install awsebcli
{% endraw %}
{% endhighlight %}

2) 설치가 완료된 후, 버전확인하기.
{% highlight html %}
{% raw %}
$ eb --version
{% endraw %}
{% endhighlight %}

다음과 같이 나오면 정상입니다.

![eb_version](https://user-images.githubusercontent.com/37537351/57964669-e7488400-7973-11e9-9fe2-f64c722fa529.png)



## 6. git ignore파일 만들기

> gitignore파일이란?
git에 올리고 싶지 않은 것들을 미리 걸러놓음.
프로젝트 전체를 올리고 싶지 않을 때나 가상환경과 같은 프로젝트 내용과는 큰 관련이 없는 것들 같은 것을
빼고 올리게 만드는 문서.

* manage.py가 있는 같은 위치에 새파일을 만들고 제목은 .gitignore이라고 한다.

{% highlight html %}
# Created by https://www.gitignore.io/api/django
# Edit at https://www.gitignore.io/?templates=django

### Django ###
*.log
*.pot
*.pyc
__pycache__/
local_settings.py
db.sqlite3
media

# Elastic Beanstalk Files
.elasticbeanstalk/*
!.elasticbeanstalk/*.cfg.yml
!.elasticbeanstalk/*.global.yml

{% endhighlight %}

* elasticbeanstalk폴더 안에 있는 파일은 모두 무시해라,
하지만, !붙은 파일(cfg.yml, global.yml)은 무시하지 말아라.

### github에 올리기
* 깃허브에 들어가서 레포지토리 생성 후
{% highlight html %}
{% raw %}
$ git init
$ git status
$ git add .
$ git commit -m "first commit"
$ 리모트 설정후, push
{% endraw %}
{% endhighlight %}

## 7. requirements.txt 만들기

1) pip freeze를 실행하고, 이름이 requirements.txt인 파일에 출력을 저장.
{% highlight html %}
{% raw %}
$ pip freeze > requirements.txt
{% endraw %}
{% endhighlight %}

* manage.py가 있는 같은 위치에 파일을 만들어야 함.
* requirements 파일은 의존성 파일로, 우리의 프로젝트가 실행될 때 필요한 설정들을 적는 파일.


## 8. .ebextensions라는 폴더 생성

1) 이름이 .ebextensions인 디렉터리를 새로 생성
{% highlight html %}
{% raw %}
$ mkdir .ebextensions
{% endraw %}
{% endhighlight %}

* manage.py가 있는 같은 위치에 폴더를 생성해야함.
* ebextensions폴더 안에 의존성 파일을 하나 작성. 파일 이름은 **django.config**라고 한다.
* **django.config** 파일 안에 다음과 같은 코드 넣어준다.
{% highlight html %}
{% raw %}
option_settings:
    aws:elasticbeanstalk:container:python:
        WSGIPath: wsgi.py가있는 폴더위치/wsgi.py
{% endraw %}
{% endhighlight %}

* WSGIPath를 작성할때, /앞에 wsgi.py가 있는 폴더 이름을 넣어준다.

그렇다면 전체 파일 및 폴더의 위치는 다음과 같습니다.
![Inked위치_LI](https://user-images.githubusercontent.com/37537351/61577331-fccb5d00-ab20-11e9-9afe-7f7c031c2609.jpg)


### github에 올리기

{% highlight html %}
{% raw %}
$ git status
$ git add .
$ git commit -m "second commit"
$ git push
{% endraw %}
{% endhighlight %}

* push후, github에 잘 올라갔는지 확인해준다.

## 9. 가상환경 끄기

{% highlight html %}
{% raw %}
$ deactivate
{% endraw %}
{% endhighlight %}

## 10. eb환경시작

1) eb init 명령으로 EB CLI 리포지토리를 초기화
{% highlight html %}
{% raw %}
$ eb init -p python-3.6 django-tutorial
{% endraw %}
{% endhighlight %}

* eb init -p python-3.6 APPLICATION이름
* 이름이 django-tutorial인 새 애플리케이션을 만들고 최신 Python 3.6 플랫폼 버전을 통해 환경을 생성하도록 로컬 리포지토리를 구성함.

### 액세스 키 만들기

AWS콘솔에 로그인 -> 내 보안자격증명 메뉴 -> 액세스 키 -> 새 액세스키 만들기 -> 액세스키 다운로드 -> 아이디와 시크릿키 입력


## 11. 장고에 맞는 환경 생성 

{% highlight html %}
{% raw %}
$ eb create django-env
{% endraw %}
{% endhighlight %}

* 장고로서 사용할 수 있는 환경을 서버 컴퓨터를 하나 할당받는 과정.
* 다음과 같이 뜨면 성공입니다.

![create_django-env](https://user-images.githubusercontent.com/37537351/57965184-c08e4b80-797b-11e9-8ca1-4c1cdd37c1f3.png)


## 12. 할당된 주소 확인

* 할당된 도메인(주소)이 무엇인지 eb status명령어를 통해 확인.
{% highlight html %}
{% raw %}
$ eb status
{% endraw %}
{% endhighlight %}

* CNAME에 해당하는 것이 주소이다.

![cname](https://user-images.githubusercontent.com/37537351/57965280-334bf680-797d-11e9-94d2-bc7500d237fd.jpg)


* CNAME에 해당하는 것을 복사해서, **setting.py**안의 **ALLOWED_HOSTS = []**에 넣어준다.

{% highlight html %}
{% raw %}
ALLOWED_HOSTS = ['CNMAE주소']
예시: 	ALLOWED_HOSTS = ['django-env.twsivis3tj.us-west-2.elasticbeanstalk.com']
{% endraw %}
{% endhighlight %}

### github에 올리기

{% highlight html %}
{% raw %}
$ git status
$ git add .
$ git commit -m "last commit"
$ git push
{% endraw %}
{% endhighlight %}

## 13. 배포하기

{% highlight html %}
{% raw %}
$ eb deploy
{% endraw %}
{% endhighlight %}

다음 사진과 같이 environment update completed successfully라고 나오면 성공입니다.

![deploy](https://user-images.githubusercontent.com/37537351/57965330-d8ff6580-797d-11e9-9f3a-dc0fff328cb5.png)

## 14. 사이트 열기

해당하는 주소를 열어볼 것인데, 이 주소를 바로 열어주는 명령어는 **eb open**입니다.

{% highlight html %}
{% raw %}
$ eb open
{% endraw %}
{% endhighlight %}

* 조금 전의 CNAME의 주소를 복사해서 URL에 붙여넣기 해도 같습니다.


## 15. 어플리케이션 삭제하기

배포가 성공적으로 끝났다면, 다시 어플리케이션을 삭제해주어야합니다.<br>
Elasticbeanstalk은 일정기간동안은 무료이지만, 그 일정기간이 지나면 과금이 됩니다.<br>
따라서, 꼭 잊지 말고 어플리케이션을 삭제해주어야 합니다.<br>
어플리케이션을 삭제해주는 명령어는 다음과 같습니다.<br>

{% highlight html %}
{% raw %}
$ eb terminate --all
{% endraw %}
{% endhighlight %}


## 16. 추가적 eb 명령어

<어플리케이션을 아예 삭지하지 않고, elastic beanstalk환경만을 종료하고 싶을때>
{% highlight html %}
{% raw %}
$ eb terminate django-env
{% endraw %}
{% endhighlight %}

* 이 명령은 환경과 그 안에서 실행되는 모든 AWS 리소스를 종료합니다.
그러나 어플리케이션은 삭제되지 않으므로 **eb create**를 다시 실행하여 동일한 구성의 더 많은 환경을 언제든 생성할 수 있습니다.
* 하지만 연습 후엔 반드시, **eb terminate --all**명령을 통하여 어플리케이션까지 삭제해야 합니다. 그렇지 않으면 **과금**이 발생할 수 있으니 주의해주세요!

<관리 콘솔 오픈하기>

{% highlight html %}
{% raw %}
$ eb console
{% endraw %}
{% endhighlight %}

* 브라우저를 열어 Elastic Beanstalk 관리 콘솔의 환경 구성 대시보드를 표시합니다.



**[ppt]**

![방학세션1_aws배포하기.pdf](https://github.com/hyojin17/session18_folder/files/3413574/1_aws.pdf)




