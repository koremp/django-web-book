# chapter 04

Django의 핵심 기능

## 4.1 Admin 사이트 꾸미기

Look and feel 제공
데이터의 CRUD


### 4.1.1 데이터 입력 및 수정

```models.py```

```python
class Question(models.Model):
    question.text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')
```

### 4.1.2 필드 순서 변경

```polls/admin.py```

```python
class QuestionAdmin(admin.ModelAdmin):
    fields = ['pub_date', 'question_text']

admin.site.register(Question, QuestionAdmin)
admin.site.register(Choice)
```

### 4.1.3 각 필드를 분리해서 보여주기

```polls/admin.py```

```python
class QuestionAdmin(admin.ModelAdmin):
    fields = [
        ('Question Statement', {'fields': ['question_text']}),
        ('Date Information', {'fields': ['pub_date']}),
    ]
```

### 4.1.4 필드 접기

```polls/admin.py```

```python
        ('Date Information', {'fields': ['pub_date'], 'casses': ['collapse']})
```

### 4.1.5 외래키 관계 화면

### 4.1.6 Question 및 Choice를 한 화면에서 변경하기

```polls/admin.py```

```python
class ChoiceInline(admin.StackedInline):
    model = Choice
    extra = 2

class QuestionAdmin(admin.ModelAdmin):
    fieldsets = [
        ('None', {'fields': ['question_text']}),
        ('Date Information', {'fields': ['pub_date'], 'classes': ['collapse']}),
    ]
    inlines = [ChoiceInline]
```

### 4.1.7 테이블 형식으로 보여주기

### 4.1.8 레코드 리스트 컬럼 지정하기

### 4.1.9 list_filter 필터

### 4.1.10 search_fields 

### 4.1.11 polls/admin.py 변경내역 정리

### 4.1.12 Admin 사이트 템플릿 수정

## 4.2 장고 파이썬 쉘로 데이터 조작하기

```bash
$ python manage.py shell
```

### 4.2.1 Create - 데이터 생성/입력

### 4.2.2 Read - 데이터 조회

### 4.2.3 Update - 데이터 수정

### 4.2.4 Delete - 데이터 삭제

### 4.2.5 polls 애플리케이션의 데이터 실습

## 4.3 템플릿 시스템

템플릿 시스템 
* MVT 방식에서 UI를 담당하고 있는 기능
* 템플릿 코드 작성 시 HTML과 장고의 템플릿 코드가 섞인다
  * 템플릿 코딩은 로직을 표현하는것이 아닌, look and feel을 표현한다.
  * 템플릿 코딩은 화면 구현이라고 하는 것이 적절한 표현.

템플릿 코드
* if 태그, for 태그, 템플릿 시스템의 고유 무ㅡㄴ법

렌더링
* 템플릿 코드를 템플릿 파일(HTML, JSON, XML 등 텍스트 파일)로 해석하는 과정

### 4.3.1 템플릿 변수

```html
{{ variable }}

{{ foo.var }}
```

### 4.3.2 템플릿 필터

필터
* 어떤 객체나 처리 결과에 추가적으로 명령을 적용하여 해당 명령에 맞게 최종 결과를 변경하는 것
* 파이프(|) 문자를 사용
 ```html
{{ name|lower }}
```
* 체인으로 연결
    * text 변수 값 중 특수 문자를 이스케이프, 결과 스트링에 HTML <p> 태그를 붙여줌
```html
{{ text|escape|linebreaks }}
```  
* 몇 가지의 필터는 인자를 가질 수 있다.
    * 30개 단어, 줄 바꿈 문자 모두 없애기
    ```html
    {{ bio|truncatewords:30 }}
    ```
    * 필터의 인자에 빈칸이 있는 경우 따옴표로 묶어주기
    ```html
    {{ list|join:" // " }}
    ```
    * value 변수값이 False이거나 없는 경우 "nothing"으로 보여주기
    ```html
    {{ value|default:"nothing" }}
    ```
    * value 변수값의 길이를 반환
    ```html
    {{ value|length }}
    ```
    * value 변수값에서 HTML 태그를 모두 없애줌
    ```html
    {{ value|striptags }}
    ```
    * 복수 접미사 필터
    ```html
    {{ value|pluralize }}
    ```
    ```html
    {{ value|pluralize: "es" }}
    {{ value|pluralize: "ies" }}
    ```
    * 더하기 필터
    ```html
    {{ value|add:"2"}}
    ```

### 4.3.3 템플릿 태그

템플릿 태그는 ```{% tag %}``` 형식을 가진다.

#### {% for %} 태그

리스트에 담겨 있는 항목들을 순회하며 출력 가능.

* forloop.counter
* forloop.counter()
* forloop.revcounter
* forloop.revcounter()
* forloop.first
* forloop.last
* forloop.parentloop

#### {% if %} 태그

```
{% if %}
{% elif %}
{% else %}
{% endif %}
```

boolean 연산자 사용 가능
```
and, or, not, and not, ==, !=, <, >, <=, >=, in, not in
```

#### {% csrf_token %} 태그

* CSRF(Cross Site Request Forgery) 공격 방지.
    * 사이트 간 요청 위조 공격
    * 공격 코드가 삽입된 페이지를 웹사이트가 공격 받게 되는 방식
* <form> 엘리먼트 첫 줄 다음에 넣어주면 된다.
* 외부 URL로 보내는 <form> 에는 사용하지 말아야 한다.

#### {% url %} 태그

소스에 URL을 하드코딩하는 것을 방ㅅ지하기 위함.

```html
<form act ion="{% url 'polls:vote' question.id %}" method="post">
```

```html
{% url 'namespace:view-name' arg1 arg2 %}
```

#### {% with %} 태그

특정 값을 변수에 저장해두는 기능

#### {% load %} 태그

사용자 정의 태그 및 필터를 로딩해준다.

### 4 .3.4 템플릿 주석

#### 한 줄 주석문

`{# #}`

```
{# greeting #}hello
```

#### 여러 줄의 주석 문

`{% comment %}`

```html
{% comment "Optional Note" %}
<p>Commented out text here</p>
{% endcomment %}
```

### 4.3.5 HTML 이스케이프

* 템플릿 코드를 렌더링하여 html 텍스트를 만들 때 템플릿 변수에 HTML 태그가 들어있으면 문제가 될 수 있음
* XSS 공격
    * Cross-Site Scripting
    * 사이트 간 스크립팅 공격
    * 웹 애플리케이션이 사용자로부터 입력받은 값을 제대로 검사하지 않고 사용할 경우 나타남.
* 장고는 디폴트로 자동 이스케이프 기능을 제공
    * HTML에서 사용되는 예약 문자를 예약 의미를 제거한 문자로 변경해주는 기능을 제공
    ```
    < &lt;
    > &gt;
    ' &39;
    " &quot;
    & &amp;
    ```

비활성화 방법
1. safe 필터 사용
safe 필터는 템플릿 변수에만 영향을 끼친다.
```
This will not be escaped: {{ data|safe }}
```

2. {% autoescape %} 태그 사용
템플릿 코드에서 범위를 정하여 이스케이프를 방지 가능.
```
{% autoescape off %}
Hello {{ name }}
{% endautoescape %}
```

필터의 인자에 사용되는 스트링 리터럴(literal)에는 자동 이스케이프 기능이 적용되지 않는다.
두 번째 문장을 사용하는 것이 좋다.
```
{{ data|default:"3 < 5" }}
{{ data|default:"3 &lt; 5}}
```

### 4.3.6 템플릿 상속

* 템플릿 문법 중 가장 복잡하지만 강력한 기능.
* 템플릿 코드 재사용 가능, 사이트의 룩앤필을 일관성 있게 보여줄 수 잇다.
* 부모 템플릿
    * 템플릿의 뼈대를 만들어준다.
    * {% block %} 태그를 통해 하위로 상속해줄 부분을 지정
* 자식 템플릿
    * 부모 템플릿의 뼈대를 그대로 재사용
    * {% block %} 부분만 채워주면 된다.
    * {% extends %} 태그를 사용하여 상속받는 다는 것을 표시
    ```
    {% extends "parent_template.html"%}
    ```

템플릿 사용 장점

* 템플릿 전체의 모습을 구조화할 수 있다.
    * 코드의 재사용/변경 용이
    * 사이트 UI의 룩앤필을 일관되게 가져갈 수 있음
    
템플릿 상속을 3단계로 사용하는 것을 권장.

1. 사이트 전체의 룩앤필을 담는 ```base.html```을 만듦
2. 사이트 하위의 섹션별 스타일을 담고있는 ```base_news.html, base_sports.html``` 등의 템플릿을 만든다. 2단계 템플릿은 1단계 ```base.html```템플릿을 상속받는다.
3. 개별 페이지에 대한 템플릿을 만든다. 3단계 템플릿들은 2단계 템플릿 중에서 적절한 템플릿을 상속받는다.

유의
* {% esxtends %} 태그는 사용하는 태그 중 가장 먼저 나와야 함 
* 템플릿의 공통 사항을 가능하면 많이 뽑아서 1단계 부모 탬플릿에 {% block %} 태그가 많아질수록 좋다.
* 부모 템플릿의 {% block %} 안에 있는 내용을 그대로 사용하고 싶다면 자식 템플릿에서 {{ block.super }} 변수를 사용하면 된다.
부모 템플릿의 내용을 그대로 사용하면서 자식 템플릿에서 내용을 추가하는 경우에 사용 가능.
* 가독성을 높이기 위하여 {% endblock content %} 처럼 블록명을 기입해도 된다.


## 4.4 폼 처리하기

### 4.4.1 HTML에서의 폼

<form>...</form> 사이에 있는 엘리먼트들의 집합.

* 폼을 통해 텍스트를 입력/항목 선택 가능
* 간단한 엘리먼트
    * 예) 텍스트 입력, 체크 박스
    * 기본 위젯 사용
* 복잡한 엘리먼트
    * 예) 달력 위젯, 슬라이드 바
    * JS, CSS 사용
* input 엘리먼트
* action 속성 지정
    * 폼 데이터를 어디로 보낼지 지정
* method 속성 설정
    * 어떤 http 메소드로 보낼지 지정
    * 폼에서 사용할 수 있는 메소드는 GET, POST 뿐
        * 장고에서는 폼은 POST 방식만을 사용

#### GET

* 서버 시스템의 상태를 바꾸는 요청
* 패스워드 폼에 사용 권장 X
    * 패스워드가 URL/브라우저 히스토리/서버의 로그에 텍스트포 보일 수 있음
* 폼 데이터량이 많거나 이미지 같은 2진 데이터 보내는 경우 부적합
* 보안 취약
* 검색 폼에 적절
    * GET 방식의 데이터가 URL에 포함
        * 북마크/공요/재전송이 편하기 때문


#### POST

* 서버 시스템의 상태를 바꾸지 않는 요청
* 패스워드 폼
* 폼 데이터량이 많거나, 이미지와 같은 2진 데이터를 보내는 경우

### 4.4.2 장고의 폼 기능

장고는 폼 처리를 위한 기능을 제공.
* 폼 생성에 필요한 데이터를 폼 클래스로 구조화
* 폼 클래스의 데이터를 렌더링하여 HTML 폼 제작
* 사용자로부터 제출된 폼과 데이터를 수신/처리

폼 클래스는 폼을 기술하고, 폼이 어떻게 작동하고 어떻게 보이는지를 결정.

폼 클래스의 필드
* HTML 폼의 <input> 엘리먼트에 매핑
* 클래스
* 폼 데이터를 저장
* 유효성 검사를 실시
* 브라우저에서 HTML 위젯으로 표현
* 필드 타입마다 디폴트 위젯 클래스를 가짐
* 필요시 오버라이딩

장고에서 객체를 렌더링할 때 다음 3가지 과정을 거침
* 렌더링할 객체를 뷰로 가져오기
* 그 객체를 템플릿 시스템으로 넘겨주기
* 템플릿 문법을 처리해서 html 마크업 언어로 변화하기

뷰 함수에서 폼 객체를 생성할 때 언바운드/바운드 폼 구분하여 코딩
언바운드 폼
* 데이터가 없는 폼
* 렌더링되어 사용자에게 보여질 때 비어있거나 디폴트 값으로 채워짐.
바운드 폼
* 제출된 데이터를 갖고있어 데이터 유효성 검사시 사용

### 4.3.3 폼 클래스로 폼 생성

* 모든 폼 클래스는 `django.forms.Form`의 자식 클래스로 생성. 
    * max_length
        * 100글자 이상 입력 방지
        * 데이터 길이 유효한지 검사
* 디폴트 위젯 변경시 폼필드 정의시 명시적으로 지정

```
a = forms.CharField(label='asdf', max_length=n, widget=forms.Textarea)
```

* 장고 폼 클래스는 `is_valid()` 메소드를 가짐
    * 유효성 검사 루틴을 실행
    * 유효시
        * True 반환
        * 폼 데이터를 cleaned_data 속성에 넣는다.

### 4.4.4 뷰에서 폼 클래스 처리



### 4.4.5 폼 클래스를 템플릿으로 변환

## 4.5 클래스형 뷰

뷰
* 요청 받아 응답 반환하는 호출 가능한 객체(callable)
* 함수형 뷰
* 클래스형 뷰
    * 상속과 믹스인 기능을 사용해서 코드를 재사용
    * 뷰를 체게젹으로 구성

### 4.5.1 클래스형 뷰의 시작점

URLconf에서 함수형 뷰가 아니라 클래스형 뷰를 사용한다는 점을 표시

```python
# urls.py

from django.urls import path
from myapp.views import MyView

urlpatterns = [
    path('about/', MyView.as_view()),
]
```

as_view() 진입 메서드
* 클래스의 인스턴스 생성
* 그 인스턴스의 dispatch() 메서드를 호출

dispatch() 메서드
* 요청 검사 후 어떤 HTTP 메서드로 요청되었는지 구분
* 인스턴스 내 해당 이름을 갖는 메서드로 요청 redirection
* 해당 메서드 미정의시 `HttpRespojnseNotAllowed` 익셉션 발생

### 4.5.2 클래스형 뷰의 장점 - 효율적인 메소드 구분

클래스형 뷰 > 함수형 뷰
* HTTP 메서드에 따른 처리 기능 코딩시 메서드명으로 구분하여 깔끔한 코드의 구조 유지
* 다중 상속같은 객체 지향 기술 가능하므로, 클래스형 제네릭 뷰 및 믹스인 클래스 사용 가능, 코드의 재사용성 및 개발 생산성 향상

### 4.5.3 클래스형 뷰의 장점 - 상속 기능 가능

제네릭 뷰
* 장고에서 뷰 개발 과정 중 공통 기능을 추상화하여 기본적으로 제공해주는 클래스형 뷰
* 상속 가능
* 오버라이딩 가능

```python
# some_app/urls.py

from django.urls. import path
from django.views.generic import TemplateView

urlpatterns = [
    path('about/', TemplateView.as_view(template_name="about_html")),
]
```

### 4.5.4 클래스형 제네릭 뷰

제네릭 뷰
* 장고에서 공통된 로직을 미리 개발해 놓고 제공하는 뷰
* 4가지로 분류 가능
    * Base View
        * 뷰 클래스 생성
        * 다른 제네릭 뷰의 부모 클래스를 제공하는 기본 제네릭 뷰
    * Generic Display View
        * 객체 리스트, 특정 객채의 상세 정보 표시
    * Generic Edit View
        * 폼을 통한 객체 생성, 수정, 삭제 기능 제공
    * Generic Date View
        * 날짜 기반 객체의 연/월/일 페이지 구분하여 표시

### 4.5.5 클래스형 뷰에서 처리

폼 처리 과정 분류
* 최초의 GET
    * 사용자에게 처음으로 폼을 보여줌
* 유효한 데이터를 가진 POST
    * 데이터 처리
    * 주로 리다이렉트 처리
* 유효하지 않은 데이터를 가진 POST
    * 에러 메시지
    * 폼 재출력

## 4.6 로그 남기기

파이썬의 로깅 모듈
* 로거
* 핸들러
* 필터
* 포맷터

장고 실행시 `settings.py` 파일에 정의된 `LOGGING_CONFIG, LOGGING` 항목을 참조하여 로깅 관련 설정 처리

한 개의 로거에 여러개의 핸들러를 저장 가능

필터 체인 가능

로거나 핸들러 양쪽에 적용 가능

### 4.6.1 로거

Logger
* 로깅 시스템의 시작점
* 로그 메시지 처리 위한 메시지를 담아두는 저장소
* 모든 로거는 이름을 갖고 있음
* 로그 레벨을 가짐

로그 레코드
* 로그에 저장되는 메시지
* 로그 레벨
* 로그 이벤트에 대한 메타 정보도 가질 수 있음
    * 스택 트레이스 정보
    * 에러 코드 등



### 4.6.2 핸들러

핸들러
* 로거에 있는 메시지에 무슨 작업을 할지 결정하는 엔진
* 로그 동작을 정의
    * 메시지 표시
    * 어디에 기록할 것인지 결정
* 로그 레벨을 가짐
    * 로그 레코드의 로그 레벨이 핸들러의 로그 레벨보다 더 낮으면 핸들러는 메시지를 무시
* 로거는 여러 개의 핸들러를 가질 수 있음
    * 각 핸들러의 서로 다른 로그 레벨 가질 수 있음
    * 메시지의 중요도에 따른 로그 처리 가능

### 4.6.3 필터

* 로그 레코드가 로거에서 핸들러로 넘겨질 때 필터를 사용하여 로그 레코드에 추가적인 제어
* 로그 처리 기준을 추가 가능
* 로그 레코드를 보내기 전에 수정하는 것도 가능
* 로거 또는 핸들러 어디에나 적용 가능
* 여러 개의 필터를 체인 방식으로 동작시킬 수 있음

### 4.6.4 포맷터

* 로그 레코드가 최종적으로 텍스트로 표현될 때 사용할 포맷을 지정
    * 파이썬의 포맷 스트링 사용
    * 사용자 정의 포맷터 사용 가능

### 4.6.5 로거 사용 및 로거 이름 계층화

장고의 로깅 설정은 settings.py 파일에서 작성

적절한 위치에서 로거 취득 후 적절한 위치에서 로깅 메소드 호출

모듈단위로 로그를 기록

```python
logger = logging.getLogger('project.interesting.stuff')
```

빈 문자열의 로거는 루트 로거(파이썬의 최상위 로거)가 됨

로그의 계층화
* 로깅 호출은 부모 로거에도 전파
* 최상단 루트 로거에서 핸들러를 만들어도 하위 로거의 모든 로깅 호출을 잡을 수 있음

### 4.6.6 장고의 디폴트 로깅 설정

장고의 settings.py 파일의 LOGGING 항목에 로깅 속성을 사전형으로 정의 (dictConfig)

### 4.6.7 장고의 로깅 추가 사항 정리



### 4.6.8 로깅 설정 - 디폴트 설정 유지

```py
# mysite/settings.py

LOGGING_CONFIG = 'logging.config.dictConfig'
```

### 4.6.9 로깅 설정 - 디폴트 설정 무시

```py
# mysite/settings.py

LOGGING_CONFIG = None

LOGGING = {
    # STH
}

import logging.config
logging.config.dictconfig(LOGGING)
```