---
layout: post 
title: 2. CSS 기초
category: session
---

세션 2 - prod. 구자룡


# CSS 기초

## 목차

1. 기본 용어 학습   
2. CSS 기본 학습
3. 학습한 내용 합치기

### 기본 용어 학습
* HTML 기본
* CSS란?
* CSS사용법
* 각 태그간 관계 용어 – 부모, 자식, 형제
* 클래스, 아이디 사용법 – 기본

#### HTML기본
* HTML은 웹화면의 구조를 잡는 역할을 함
* <이름> 내용 </이름>
* 위와 같은 구조를 사용 하며 <이름> 태그라고 부름
* 태그 안에는 또다른 태그를 넣을 수 있음

### CSS란?
* CSS (Cascading Style Sheets)는 마크업 언어가 실제 표시되는 방법을 기술하는 언어
* 쉽게 말해서 HTML을 꾸며 주며 주는 역할
* HTML태그에 속성을 주어 꾸며주거나 태그에 CLASS 혹은 ID를 주어 꾸밀 수 있음

![1](https://user-images.githubusercontent.com/30469948/54490928-d17bfb80-48fd-11e9-8233-c08b89a9295a.png)


### CSS로 할 수 있는 것은?
* CSS를 이용하면 화면에 원하는 형태로 무엇이든 만들 수 있음
* 정적인 화면은 무엇이든 제작가능
* 동적인 화면에 경우 JavaScript의 도움이 필요

![2](https://user-images.githubusercontent.com/30469948/54490929-d17bfb80-48fd-11e9-8ade-5c164d7c8439.png)

 <br>
<br>
<br>

### CSS 사용법
* CSS사용법에는 크게 3가지가 있음
1. 사용하고자 하는 테그에 style을 이용하여 사용하는 법
![3](https://user-images.githubusercontent.com/30469948/54490930-d2149200-48fd-11e9-9e8c-d06dbb198b72.png)

2. 상단 혹은 하단에 style태그로 사용
![4](https://user-images.githubusercontent.com/30469948/54490931-d2149200-48fd-11e9-96cc-a01ac88d7269.png)
3. css파일을 따로 만들어서 불러 오는 방식
![5](https://user-images.githubusercontent.com/30469948/54490932-d2149200-48fd-11e9-9362-f10e83293c7b.png)

### 각 태그간 관계 용어
> 부모
>> 상위 태그
 >자식
>> 하위 태그
 > 형제
>> 동일 선상의 태그
### 예제
![6](https://user-images.githubusercontent.com/30469948/54490933-d2ad2880-48fd-11e9-9d7c-6dfc968205d1.png)  
* 란선(naver_btn)를 기준으로 초록선은 자식 태그
* 빨간선(naver_btn_con)는 부모 태그
* 초록선 끼리는 형제 태그
![7](https://user-images.githubusercontent.com/30469948/54490934-d2ad2880-48fd-11e9-8481-4a8e9fa02d80.png)
<br>
### 클래스
* 클래스는 html 태그에 속성을 주기 위해 사용하는 이름
* Class=“이름” 형태로 사용
* 여러 개를 중복해서 줄 수도 있음
![8](https://user-images.githubusercontent.com/30469948/54490935-d2ad2880-48fd-11e9-80b8-6d99d599bf6c.png)
<br>
<br>

### 실습 준비
https://github.com/kujarung/css_project - 파일 다운로드
<br><br>
![9](https://user-images.githubusercontent.com/30469948/54490936-d345bf00-48fd-11e9-9204-6f70a690a055.png)

### 클래스 사용법
1. 바꾸고자 하는 태그에 CLASS=“이름”을 적어 넣는다.
2. .이름{내용} 과 같이 사용한다.
![10](https://user-images.githubusercontent.com/30469948/54490937-d345bf00-48fd-11e9-9fd6-a3d43b857d42.png)

### 아이디
* 아이디에 경우 페이지에서 유일하게 가지는 이름
* 보통 아이디에 경우 CSS로 꾸미는 용도로 사용하지 않음
* 개발에 필요한 경우 id값을 사용 하기 때문에 보통 id에 css를 주지 않음
* Id에 경우 .을 이용했으나 id는 #을 이용

## CSS 기본 학습

### 글자 다루기 - font
* 글자에 적용 할 수 있는 속성으로 4가지를 다룰 예정
* font-size, color, font-weight, line-height
* 이외에 letter-spacing, word-spaceing 등이 있음
* 참고로 강제 개행 br태그가 있음

 > font-size
>> 영어 그대로 글자의 크기를 조절하는 하는 속성
 > Color
>> 글자 색을 정함
 > line-heght
>> 쉽게 말해서 행간 높이를 정함
### 실습
![11](https://user-images.githubusercontent.com/30469948/54490938-d345bf00-48fd-11e9-8c8a-1cb916576ae8.png)

![12](https://user-images.githubusercontent.com/30469948/54490939-d345bf00-48fd-11e9-825a-bdd3036b7acc.png)

### 이미지 다루기 – img태그
* img태그는 웹화면에 img를 첨부하는 것
* img태그 형태로 사용하고 src에 경로 값을 넣어준다.
* alt값은 선택 사항이고 이미지가 없을 시 이미지에 대한 대체 택스트를 넣어주는 속성이다.

### 실습
![13](https://user-images.githubusercontent.com/30469948/54490940-d3de5580-48fd-11e9-9534-f11db631fa5b.png)  
![14](https://user-images.githubusercontent.com/30469948/54490941-d3de5580-48fd-11e9-8483-11c84520c815.png)  

### 배치하기 - display
* HTML은 기본적으로 위에서 아래로 쌓임
* 그 이유는 대부분의 태그의 display속성이 block이기 때문

![15](https://user-images.githubusercontent.com/30469948/54490942-d3de5580-48fd-11e9-8ac9-f0380553e0df.png)

### block속성
* Block은 대부분 HTML 태그의 기본값
* 기본적으로 브라우저(크롬 , ie , 파이어폭스 등등…)의 가로길이 전체 길이를 기본값으로 가지고 있음
* 높이에 경우 안에 들어가는 내용에 따라 자동으로 변함
* 만약 가로 길이나 높이를 지정 하고 싶다면 width, height값을 이용하여 지정 가능

### inline속성
* block이 아닌 inline에 경우 좌에서 우로 쌓임
* block과 다르게 width와 heigh값을 가질 수 없음
* 대표적으로 span태그와 img태그가 있음
* Inline에 경우 block 안에 강조를 하는 경우 주로 사용
![16](https://user-images.githubusercontent.com/30469948/54490943-d3de5580-48fd-11e9-9a6e-2d208ac45207.png)

### 공간 다루기 - margin
* margin은 나와 다른 태그 사이에 공간을 벌리는 역할
* 상하좌우로 공간을 줄 수 있음

### 공간 다루기 – padding
* padding은 내 태그 안에 공간을 주는 것
* margin과 마찬 가지로 상하좌우로 줄 수 있음

### 윤곽선 border
* 윤곽선을 그리는 걸로 border라는 속성이 있음
* 상하좌우로 줄 수 있음
* 윤곽선의 굴곡을 줄 수 있는 데 border-radius를 이용하여 줄 수 있음

### 실습
![17](https://user-images.githubusercontent.com/30469948/54490944-d476ec00-48fd-11e9-9d70-cc59cb5af35d.png)

### 공간 다루기 margin vs  padding
* 둘의 차이는 대표적으로 border와 background-image로 볼 수 있음

 * margin은 나와 상대 공간을 띄우기 때문에 margin 전에 border와 background-image 가 생김

 * padding에 경우 내 안에 공간을 주는 것이기 때문에 paddin다음에 borde와 background-image가 생김

 * 마진에 경우 패딩과 다르게 –값을 줄 수 있음 이를 네거티브 마진이라 함

### 공간 배치 하기 float
* float속성은 영어 그대로 띄운다는 의미
* float:left 혹은 right로 사용
![18](https://user-images.githubusercontent.com/30469948/54490945-d476ec00-48fd-11e9-92bd-253be2c19a18.png)  
![19](https://user-images.githubusercontent.com/30469948/54490946-d476ec00-48fd-11e9-8faa-9f816c5dcff5.png)  

### 공간 배치 하기 position
* position에는 크게 4가지를 사용
* Absolute, relative, fixed, static
* 지정하지 않으면 기본 값은 static

### 실습
![20](https://user-images.githubusercontent.com/30469948/54490947-d50f8280-48fd-11e9-9b0c-25db5fecdc43.png)  
![21](https://user-images.githubusercontent.com/30469948/54490948-d50f8280-48fd-11e9-8ad2-d074ba771478.png)  
![22](https://user-images.githubusercontent.com/30469948/54490949-d50f8280-48fd-11e9-8d85-9f87e9b475d0.png)  

### 배운것 합치기 및 응용

#### 버튼만들기
![23](https://user-images.githubusercontent.com/30469948/54490950-d50f8280-48fd-11e9-9667-1b5bdbb182df.png)


#### 버튼만들기
![24](https://user-images.githubusercontent.com/30469948/54490951-d5a81900-48fd-11e9-9f54-d784aa19ee97.png)

