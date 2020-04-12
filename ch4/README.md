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

## 4.5 클래스형 뷰

## 4.6 로그 남기기