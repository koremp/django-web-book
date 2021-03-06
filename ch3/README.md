# chapter 03

MVC 패턴에 해당하는 MVT 패턴에 따라 개발하도록 설계

## 3.1 일반적인 특징

* MVC 패턴 기반 MVT
* 객체 관계 매핑
* 자동으로 구성되는 관리자 화면
* 우아한 URL 설계
* 자체 탬플릿 시스템
* 캐시 시스템
* 다국어 지원
* 풍부한 개발 환경
* 소스 변경사항 자동 반영

## 3.2 장고 프로그램 설치

### 3.2.1 윈도우에서 장고 설치

### 3.2.2 리눅스에서 pip 프로그램으로 설치

### 3.2.3 기존 장고 프로그램 삭제

### 3.2.4 수동으로 설치

### 3.2.5 장고 프로그램 설치 확인

## 3.3 장고에서의 애플리케이션 개발 방식

애플리케이션
* 웹 사이트의 전체 프로그램 또는 모듈화된 단위 프로그램을 
* 웹 사이트 설계시 가장 먼저 해야 할 일
  * 프로그램의 모듈화

* 장고에서의 용어
  * 프로젝트
    * 웹 사이트에 대한 전체 프로그램
  * 애플리케이션
    * 모듈화된 단위 프로그램
  * 애플리케이션 프로그램들이 모여서 프로그젝트를 개발하는 개념.

### 3.3.1 MVT 패턴

MVC(Model-View-Controller) 패턴
* Model: 데이터
* View: 사용자 인터페이스
* Controller: 데이터를 처리하는 로직
* MVC를 구분하여 한 요소가 다른 요소들에게 영향을 주지 않도록 설계하는 방식
* UI 디자이너 - 개발자의 업무 구분이 가능하다.


장고의 MVC는 MVT(Model-View-Template) 패턴이라고 한다.
* Model: 데이터베이스에 저장되는 데이터
* Template(MVC-View): 사용자에게 보여주는 UI
* View(MVC-Controller): 프로그램 로직이 동작하여 데이터를 가져오고 적절하게 처리한 결과를 템플릿에 전달하는 역할을 수행
* 예시
  * 모델
    * 블로그의 내용을 데이터베이스로부터 가져오고, 저장, 수정하는 기능
    * DB와 ORM
  * 뷰
    * 버튼을 눌렀을 때 어떤 함수를 호출하며 데이터를 어떻게 가공할 것인지 결정하는 역할
    * 웹 클라이언트와 Request, Response
    * Model CRUD
  * 템플릿 
    * 화면 출력을 위해 디자인과 테마를 적용해서 보여지는 페이지를 만들어주는 역할
    * View에게 Template Rendering

장고에서 MVT 패턴에 따라 처리하는 과정
* 클라이언트로부터 요청을 받으면 `URLconf`를 이용하여 URL을 분석
* URL 분석 결과를 통해 해당 URL에 대한 처리를 담당할 `뷰`를 결정
* 뷰는 자신의 로직을 실행하며 만일 DB 처리가 필요하면 `모델`을 통해 처리후 결과 반환
* 뷰는 자신의 로직 처리가 끝나면 `템플릿`을 사용하여 클라이언트에 전송할 HTML 파일을 생성
* 뷰는 최종 결과로 HTML 파일을 클라이언트에 보내 응답


### 3.3.2 Model - DB 정의

* 모델
  * 사용될 데이터에 대한 정의를 담고 있는 장고의 클래스
  * 장고는 ORM 기법을 사용하여 하나의 모델 클래스를 하나의 테이블에 매핑되고, 모델 클래스의 속성은 테이블의 컬럼에 매핑됨.
* ORM
  * Object-Relational Mapping
  * 객체와 관계형 DB를 연결해주는 역할
  * 장점
    * 애플리케이션에서 DB 액세스를 SQL 없이도 클래스 다루듯이 할 수 있어 편리
    * 필요에 따라 DB 엔진 쉽게 변경 가능(ORM API 변경 필요 없음)

`models.py`파일에 모델 클래스를 정의한다.

### 3.3.3 URLconf - URL 정의

장고는 request를 받으면 요청이 들어있는 URL을 분석한다.
URL이 urls.py에 정의된 URL 패턴과 매칭이 되는지 분석한다.

파이썬의 URL 정의방식이 기존 자바, PHP 계열의 URL보다 직관적이며 이해하기 쉬워 우아한 URL이라고 부른다.

`urls.py` 파일에 URL과 처리 함수(뷰)를 매핑하는 파이썬 코드를 작성한다. 이러한 URL/뷰 매핑을 URLconf라고 함.

URL 분석하는 순서
* setting.py 파일의 URLconf


### 3.3.4 View - 로직 정의

### 3.3.5 Template - 화면 UI 정의

### 3.3.6 MVT 코딩 순서

* 프로젝트 뼈대 만들기
  * 프로젝트 및 앱 개발에 필요한 디렉토리와 파일 생성
* 모델 코딩하기
  * 테이블 관련 사항을 개발
  * models.py, admin.py
* URLconf 코딩하기
  * URL 및 뷰 매핑 관계를 정의
  * urls.py
* 템플릿 코딩하기
  * 화면 UI 개발
  * templates/ 디겔토리 하위의 *.html 파일들
* 뷰 코딩하기
  * 애플리케이션 로직 개발
  * views.py 파일

## 3.4 애플리케이션 설치하기

## 3.5 프로젝트 뼈대 만들기

### 3.5.1 프로젝트 생성

```bash
$ dajango-admin startproject [project_name]

```

### 3.5.2 애플리케이션 생성

```bash
$ cd [project_name]
$ python manage.py startapp [application_name]
```

### 3.5.3 프로젝트 설정 파일 변경

```bash
$ vi settings.py
```

```python
# ALLOWED_HOST, default=localhost
ALLOWED_HOST = ['host-server-ip', 'localhost', '127.0.0.1']

# INSTALLED APPS
INSTALLED_APPS = [
  '',
  '',
  '[application-name].apps.PollsConfig',
]

# DB default=SQLite3
DATABASES = {}

# TIMEZONE, default=UTC
TIME_ZONE = 'Asia/Seoul'
```

### 3.5.4 기본 테이블 생성

```bash
$ python manage.py migrate
```

### 3.5.5 지금까지 작업 확인하기

```bash
$ python manage.py runserver 0.0.0.0:8000
$ python manage.py createsuperuser
```

## 3.6 애플리케이션 개발하기 - Model 코딩

### 3.6.1 테이블 정의

```bash
$ vi [application_name]/models.py
```

### 3.6.2 Admin 사이트에 테이블 반영

```
$ vi [application_name]/admin.py
```

### 3.6.3 데이터베이스 변경사항 반영

```bash
python manage.py makemigrations
python manage.py migrate
```

### 3.6.4 지금까지 작업 확인하기

```
http://127.0.0.1:8000/admin
```

## 3.7 애플리케이션 개발하기 - View 및 Template 코딩

### 3.7.1 URLconf 코딩

```urls.py```의 urlpatterns를 추가한다.

### 3.7.2 뷰 함수 index() 및 템플릿 작성

### 3.7.3 뷰 함수 detail() 및 폼 템플릿 작성

### 3.7.4 뷰 함수 vote() 및 리다이렉션 작성

### 3.7.5 뷰 함수 results() 및 템플릿 작성

### 3.7.6 지금까지 작업 확인

