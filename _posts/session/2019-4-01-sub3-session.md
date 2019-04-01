---
layout: post
title: Python-docx(파이썬으로 word문서 만들기)
category: session
---
예비 세션 3 - prod. 최효진

python-docx(파이썬으로 word문서 만들기)

# Python-docx만들기

## 0. python docx란?
**python-dox**: 파이썬을 이용하여 MS-Word(DOC, DOCX Format) 문서를 작성 할 수 있는 라이브러리
*  문서 생성, 페이지 생성 및 설정, Table, 이미지 추가등의 작업을 수행 할 수 있다. 
* Python-Docx을 이용하여 프로그램을 통해 계산되는 데이터를 DOC Format의 정형화된 보고서 형태로 작성 할 수 있어 매우 편리하다. 
* 하지만 라이브러리의 사용방식이 기존의 다른 파이썬 및 프로그래밍 모듈의 호출 방법과 상이하며 
  라이브러리에서 사용자가 원하는 형태로 문서를 작성하는것에 제한이 따르고 글꼴, 문단모양등의 
  문서의 세부 양식은 MS-Word의 Style을 활용해야 하는 등 진입장벽이 높고 기능이 제한적이어서 
  해당 라이브러리를 이용하기 위해서는 어느정도 숙련 과정이 필요하다.


## 1. python docx를 만들기 위한 환경 만들기

* vs code에 가상환경을 만든다.
(이 과정은 꼭 실행하진 않아도 된다.)
{% highlight html %}
$ python -m venv myvenv #가상환경 만들기
$ source 가상환경이름/Scripts/activate #가상환경 실행하기
{% endhighlight %}

* 라이브러리 다운받기
{% highlight html %}
$ pip install python-docx
{% endhighlight %}

* 파일 만들기
(원하는파일명.py)를 만든다.
ex) docx_practice.py

* import설정
python-docx기능을 사용하기 위해, 파일의 가장 위쪽에 **from docx import Document** 를 import한다.

{% highlight html %}
from docx import Document
{% endhighlight %}

## 2. 제목과 문단 만들기

#### 2-1) 시작하기

사용하기위한 변수선언, 클래스처럼 Document()를 document변수에 넣는다.
{% highlight html %}
document = Document()
{% endhighlight %}

#### 2-2) 제목만들기

**document.add_heading('제목명', 0)** 을 통해서 제목을 넣을 수 있다.

{% highlight html %}
document.add_heading('python-docx만들기', 0)
{% endhighlight %}


![제목생성](https://user-images.githubusercontent.com/37537351/55269623-e468d580-52d8-11e9-9594-cd957a4f0748.PNG)

#### 2-3) 문단 만들기

* 본문은 **add_paragraphs('')** 함수를 이용해 입력해 준다.
  
{% highlight html %}
p = document.add_paragraph('안녕하세요, 저에 대해서 ')
{% endhighlight %}

* 이후 같은 paragraphs에 입력을 원할경우 add_run() 함수를 사용한다.
{% highlight html %}
p.add_run('자기소개').bold = True
p.add_run('를 해보겠습니다. ')
p.add_run('by. hyojin').italic = True
{% endhighlight %}

![본문작성](https://user-images.githubusercontent.com/37537351/55269673-707afd00-52d9-11e9-89a9-c1486470d65b.PNG)


## 3. 폰트

#### 3-1) 글자 크기

기본적으로 Word에서는 '제목 1'로 나타나는 최상위 제목을 추가합니다.
하위 섹션의 제목을 원하면 1과 9 사이의 정수로 원하는 수준을 지정하면 된다.

{% highlight html %}
document.add_heading('인적사항', level=1)
document.add_heading('이름과 나이', level=2)
document.add_heading('heading3', level=3)
document.add_heading('heading4', level=4)
{% endhighlight %}

![제목글씨크기](https://user-images.githubusercontent.com/37537351/55269775-889f4c00-52da-11e9-880b-d2a026aa01ff.PNG)


#### 3-2) 인용문
인용문스타일을 사용하기 위해, **add_paragraph('작성하고자하는 내용', style='Intense Qute')** 로 스타일을 지정해준다.

{% highlight html %}
document.add_paragraph('Intense quote', style='Intense Quote')
{% endhighlight %}

![인용문](https://user-images.githubusercontent.com/37537351/55269792-b7b5bd80-52da-11e9-9f44-a4235b352f13.PNG)

#### 3-3) bold와 itatlic체
내용을 굵게(bold), 기울임(italic)하고 싶다면 **.bold = Ture** 와 **.italic = True** 를 사용한다.

{% highlight html %}
p.add_run('자기소개').bold = True
p.add_run('를 해보겠습니다. ')
p.add_run('by. hyojin').italic = True
{% endhighlight %}


![bold italic](https://user-images.githubusercontent.com/37537351/55269859-593d0f00-52db-11e9-996b-c218353a3710.PNG)


## 4. 리스트

#### 4-1) 순서가 없는 리스트
순서가 없는 리스트를 사용하고 싶다면, style을 **style='List Bullet'** 으로 설정한다.

{% highlight html %}
document.add_paragraph('이름: 최효진', style='List Bullet')
{% endhighlight %}

다음과 같이도 사용가능하다.

{% highlight html %}
lists = document.add_paragraph('나이: 22세')
lists.style = 'List Bullet'
{% endhighlight %}

![순서없는리스트](https://user-images.githubusercontent.com/37537351/55269898-c2248700-52db-11e9-9fcf-dd190a1c3d99.PNG)

#### 4-2) 순서가 있는 리스트
순서가 없는 리스트를 사용하고 싶다면, style을 **style='List Number'** 으로 설정한다.

{% highlight html %}
document.add_paragraph('전공: Global Business & Technology학부', style='List Number')
document.add_paragraph('학년: 3학년', style='List Number')
{% endhighlight %}

![순서있는리스트](https://user-images.githubusercontent.com/37537351/55269899-d4062a00-52db-11e9-963f-f85b368d3cc6.PNG)


## 5. 이미지

#### 5-1) 이미지 넣기
업로드 하고 싶은 이미지 파일을 docx_practice.py가 있는 폴더에 넣고, 
**add_picture('이미지파일이름.확장자')** 로 작성한다.

{% highlight html %}
document.add_picture('image-filename.png')
{% endhighlight %}

#### 5-2) 이미지 사이즈
이미지 사이즈는 단위 Inches와 Cm 등 여러 단위를 사용가능하다.
이미지 사이즈 지정을 위한 단위를 사용하기 위해, 다음을 파일의 위에 import해준다.

{% highlight html %}
from docx.shared import Cm, Inches
{% endhighlight %}


원하는 사이즈를 width-넓이와, height-높이의 ()괄호안에 넣어준다.
{% highlight html %}
document.add_picture('mung.png', width=Inches(1.25))
document.add_picture('cat.jpg',width= Cm(7), height= Cm(4))
{% endhighlight %}


## 6. 표(table) 작성

#### 6-1) 표 생성
{% highlight html %}
table = document.add_table(rows=1, cols=3)
table.style = document.styles['Table Grid']
{% endhighlight %}

테이블의 row(행)과 cols(열)수만큼 숫자를 지정해준다.
테이블의 스타일을 []안에 넣는데, Table Grid는 테이블 기본 스타일이다.

#### 6-2) 테이블 스타일
다른 스타일의 테이블을 생성하고 싶다면, []안에 디폴트로 생성되어 있는 테이블 중 원하는 테이블명을 넣으면 된다.
{% highlight html %}
table.style = document.styles['Colorful Grid Accent 1']
{% endhighlight %}

'Table Grid'가 테이블 기본 스타일이라면, 'Colorful Grid Accent 1'은 색깔이 있는 테이블이다.
디폴트테이블의 명은 python-docx의 공식문서 중 디폴트 템플릿의 가장 아래부분에 있다.

<https://python-docx.readthedocs.io/en/latest/user/styles-understanding.html#table-styles-in-default-template>

![테이블스타일](https://user-images.githubusercontent.com/37537351/55269999-27c54300-52dd-11e9-9b3a-112761cbf9f6.PNG)


#### 6-3) 테이블 데이터 작성

{% highlight html %}
records = (
    (1, '육회', '5'),
    (2, '연어초밥', '4'),
    (3, '김치볶음밥', '3')
)
{% endhighlight %}

#### 6-4) 셀 설정
{% highlight html %}
hdr_cells = table.rows[0].cells
hdr_cells[0].text = 'rank'
hdr_cells[1].text = 'name'
hdr_cells[2].text = 'agree'
{% endhighlight %}


#### 6-5) 테이블 데이터 넣기

{% highlight html %}
for rank, name, agree in records:#for문 돌면서
    row_cells = table.add_row().cells#셀추가하기
    row_cells[0].text = str(rank)#rank는 int형이기 떄문에 str로 형변환을 해준다.
    row_cells[1].text = name
    row_cells[2].text = agree
{% endhighlight %}

![테이블내용](https://user-images.githubusercontent.com/37537351/55270047-9d311380-52dd-11e9-996c-7aa217f0bdbe.PNG)


#### 6-6) 셀 추가하기
**table.add_row()** 를 이용해 기본 테이블에서 셀을 추가해준다.

{% highlight html %}
row_cells = table.add_row().cells
row_cells[0].paragraphs[0].add_run('4').bold = True
row_cells[1].paragraphs[0].add_run('삼겹살').bold = True
row_cells[2].paragraphs[0].add_run('5').bold = True 
{% endhighlight %}

![셀추가](https://user-images.githubusercontent.com/37537351/55270085-20526980-52de-11e9-9734-07b421c93177.PNG)

#### 6-7) 가운데 정렬
중앙정렬을 하기 위해서, WD_ALIGN_PARAGRAPH를 import해줘야 한다.
{% highlight html %}
from docx.enum.text import WD_ALIGN_PARAGRAPH
{% endhighlight %}

**.alignment = WD_ALIGN_PARAGRAPH.CENTER**를 이용해 가운데 정렬을 한다.

{% highlight html %}
row_cells = table.add_row().cells
row_cells[0].paragraphs[0].add_run('4').bold = True
**row_cells[0].paragraphs[0].alignment = WD_ALIGN_PARAGRAPH.CENTER**
row_cells[1].paragraphs[0].add_run('삼겹살').bold = True
**row_cells[1].paragraphs[0].alignment = WD_ALIGN_PARAGRAPH.CENTER**
row_cells[2].paragraphs[0].add_run('5').bold = True 
**row_cells[2].paragraphs[0].alignment = WD_ALIGN_PARAGRAPH.CENTER**
{% endhighlight %}

![가운데정렬](https://user-images.githubusercontent.com/37537351/55270107-84752d80-52de-11e9-950c-d0c74882c6b8.PNG)


#### 6-8) 셀 병합
병합은 두 셀 값을 받아 사각형 모양으로 병합이 된다.
병합하고자하는 **table.coll(행, 열)** 을 두 변수에 담아, **합치고자하는셀1.merger(합치고자하는셀2)** 를 통해 병합한다.

{% highlight html %}
t_sum_a = table.cell(4, 1)
t_sum_b = table.cell(4, 2)
t_sum_a.merge(t_sum_b)
{% endhighlight %}

![셀병합](https://user-images.githubusercontent.com/37537351/55270143-ea61b500-52de-11e9-971e-9205ee585d69.PNG)


## 7. 새로운 페이지 만들기

{% highlight html %}
document.add_page_break()
{% endhighlight %}

## 8. 파일 저장

{% highlight html %}
document.save('demo.docx')
{% endhighlight %}


ppt참고

[예비세션.pdf](https://github.com/hyojin17/subsession_python_docx/files/3027588/default.pdf)


