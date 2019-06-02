# Flask Web Framework (c9.io 기준)





===============================================================================

# c9.io 에서 Flask를 통해 웹사이트 구축하기

### - 기본 세팅:

- `c9.io` 에 로그인을 한 후, 신규 Workspace를 생성한다.
- bash terminal에서의 디폴트 working directory는 `/home/ubuntu/workspace` 이다. 하나의 workspace에서는 여러 개의 개별 "프로젝트"를 생성할 수 있는데, 프로젝트는 각각 `/workspace` 하위에 추가 폴더를 생성하고 그 안에서 관리한다.
- 각 프로젝트 폴더 안에는 기본적으로 `app.py` 라는 파일과, `templates` 라는 폴더를 만든다.
  - `app.py` 는 세부 URL 라우팅 정보를 설정해주고,
  - `templates` 폴더 안에 생성한 html 파일들은 필요 시 세부 URL과 매칭시켜 웹페이지를 구현한다.

### - `app.py` 파일 초기 설정

아래의 코드를 파일 상단에 저장한다:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")             # 디폴트 페이지
def index():                # 일반적으로 함수명을 index라고 한다
    return "Hello World"    # 디폴트 페이지 접속 시 실행되는 부분
```

### - bash terminal에 입력할 기본 명령어

- `sudo pip3 install flask`    ==> `import flask`  을 사용하기 위해 설치 필요
  - 다른 패키지도 import할 경우, 위와 동일한 방식으로 설치해야 할 수도 있다.
- `cd` 명령어로 현재 작업하고 있는 프로젝트 폴더(디렉토리)로 이동한다.
- `FLASK_APP=app.py flask run --host=0.0.0.0 --port=8080`  ==> 처음 한 번만 실행.
  - 그 다음부터는 `flask run --host=0.0.0.0 --port=8080` 로 서버를 구동하고,
  - 서버 끌 때는 Ctrl+C 입력
  - `app.py` 파일이나 다른 `html` 파일 수정사항을 반영하고 싶을 경우 매번 서버를 재가동해야 함.
- 서버 실행한 이후 다음의 주소에서 웹사이트에 접속 가능함:
  - https://*[workspace_name]*-whitejcme.c9users.io

### - 그 이후의 작업 및 참고 사항

- `app.py` 파일에서 `@app.route("/~~")` 를 통해 세부 웹페이지를 추가해나간다.
- 다른 패키지를 import할 경우, `app.py` 파일 상단에 추가해준다.
- `templates` 폴더의 html파일들은 기본적으로 Jinja2 라는 템플릿 엔진을 통해 렌더링된다.

.

### c9에서 alias 추가하기

1. `c9 ~/.bashrc` 로 bashrc 문서를 연다.

2. 문서에 다음과 같이 단축하고자 하는 내용을 추가한다:

   ```bash
   alias gs="git status"
   alias gcm="git commit -m"
   alias gp="git push"
   alias gl="git log"
   alias p3="python3"
   ```

3. `source ~/.bashrc` 로 bashrc 문서를 다시 로드한다.

.

### c9.io 사용하지 않고 local host로 Flask 돌리기

1. Visual Studio Code에서 Flask `app.py` 에 쓸 코드를 파이썬 파일로 생성한다.
   - `from flask import Flask` 등등
2. 해당 파이썬 파일을 실행한다. (`python3 example.py`)
3. 웹브라우저에서 `localhost:5000` 로 이동한다.
- Note) 위 방법을 쓸 경우, `app.py` 끝 부분에 다음의 코드를 추가하면 파일 내 변동 사항이 생길 때마다 웹페이지가 자동으로 새로고침되어 업데이트된다:

  ```python
  if __name__ == "__main__":
      app.run(debug=True)
  ```

  - (C9.io에서는 사용 불가)





===============================================================================

# ROUTING

# `@app.route()` 로 세부 웹페이지 구현하기

### - 세부 웹페이지 생성 시, `app.py` 파일에 다음의 기본 코드를 추가한다:

```python
@app.route("/[세부페이지URL]")  # -> decorator
def 함수명(인자):
    [함수 내용]
    return [반환값]
```

- 예) `flask` 라는 workspace에서  `@app.route("/name")` 으로 세부 웹페이지를 생성했다면, `https://flask-whitejcme.c9users.io/name` 로 접속했을 때 [반환값]에 지정된 웹페이지 구현 결과물을 확인할 수 있다.

### - return + 텍스트

```python
@app.route("/")
def index():
    return "Hello World"
```

- https://~~c9users.io/ 로 접속하면 "Hello World" 가 그냥 플레인 텍스트로 출력된다.
- *) 일반적으로 root인 `@app.route("/")` 에 해당하는 함수명으로는 index()를 사용하나, 반드시 따라야 할 사항은 아니다. 같은 `app.py` 파일 내에서 함수명이 중복되지만 않는다면 함수명은 크게 상관 없다.
  - 메인 웹페이지 이외의 세부 페이지들은 일반적으로 url 끝부분과 함수명을 동일하게 작명한다.
- 반환값의 자료형은 str이어야 한다. 다른 자료형은 `str()` 내장함수로써 형변환을 해준다.

### - URL을 통해 데이터 받기 & 웹페이지로 데이터 반환하기

```python
@app.route("/hello/<name>")
def hello(name):
    return "hello " + name
```

- 브라우저 주소창에서 `https://~~c9users.io/` 다음에 오는 텍스트가 'name' 이라는 변수에 저장되어, hello라는 함수에 인자로 전달된다.

- 예) 브라우저에서 `https://flask-whitejcme.c9users.io/name/jiwon` 로 접속하면, "jiwon"이 name에 저장되어 브라우저 화면에는 "hello jiwon"이 출력된다.

- 참고) 주소창으로 받게 될 데이터는 기본적으로 str 자료형으로 저장된다. 만약 int 자료형으로 저장하고 싶다면 `<int:num>` 으로 자료형을 명시하면 된다.

  ```python
  @app.route("/cube/<int:num")
  def cube(num):
      return num + " cubed is: " + str(num**3)
  ```

- return 부분에서 조건문도 사용할 수 있다

  ```python
  @app.route("/palindrome/<word>")
  def ispalindrome(word):
      if word == word[::-1]:
          return word + " is a palindrome"
      else:
          return word + " is not a palindrome"
  ```

### - return + send_file

```python
from flask import send_file

@app.route("/profile")
def profile():
    return send_file('profile.html')
```

- `profile.html` 파일을 브라우저에서 연다.
- 단, `profile.html` 파일은 `app.py` 파일과 동일한 디렉토리에 위치해야 한다. (`templates` 폴더 안에 있으면 에러 발생)

### - return + render_template

```python
##### app.py 파일 내용

from flask import render_template
import random

@app.route("/lotto")
def lotto():
    name = "jiwon"
    result_py = str(sorted(random.sample(range(1, 46), 6)))
    return render_template('lotto.html', result_html=result_py, name=name)
```

- lotto()함수 내에서 생성된 변수를 렌더링할 html파일로 넘길 경우, `render_template()` 의 추가 인자로 지정하면 된다.
- `result_html=result_py` 에서 보이듯 좌측은 html파일 내에서 사용되는 변수명이고, 우측은 app.py파일에서 사용되는 변수명이다. 
  - 편의를 위해 일반적으로는 `name=name` 과 같이 변수명을 통일한다.

```html
{# ### lotto.html (templates 폴더 안) 파일 내용 ### #}

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
</head>
<body>
    <h1>Automatically Generated Lotto Code for {{name}}</h1>
    <h2>{{ result_html }}</h2>
</body>
</html>
```

.

### - `<form>` 을 통해 사용자로부터 입력 받은 데이터 활용하기

`app.py` 파일 내용:

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/")
def index():
    return render_template('index.html')  #_(1)_#

@app.route("/sendmsg")
def sendmsg():
    msg = request.args.get('msg')    #_(2-3)_#
    return render_template('sendmsg.html', msg=msg)  #_(2-4)_#
```

- (1) https://flask-whitejcme.c9users.io/ 로 접속하면 index.html 이 렌더링된다.
- (2-3) 이때 이전 페이지의 form에서 'msg'라는 name 라벨이 붙은 요소를 'msg' 변수에 저장한다.
- (2-4) sendmsg.html 이 렌더링된다.



`index.html` 파일 내용:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Messenger</title>
</head>
<body>
    <h1>Enter Message:</h1>
    <form action="/sendmsg">  #_(2-2)_#
        <input type="text" name="msg">  #_(2-1)_#
        <input type="submit" value="Send">
    </form>
</body>
</html>
```

- (2-1) 사용자가 input란에 입력 후 submit하면, 입력된 데이터는 'msg'라는 name 라벨이 붙고
- (2-2) https://flask-whitejcme.c9users.io/sendmsg 페이지로 리디렉트된다.
  - 이때 form에서 데이터는 디폴트로 'GET' 방식으로 넘어가기 때문에, 위 URL 끝에는 '?msg=*[입력된데이터]*'가 추가된다.
  - 예) 사용자가 input 부분에 'abcd' 라는 텍스트를 입력할 경우, 제출 버튼 클릭 시 자동으로 'https://flask-whitejcme.c9users.io/sendmsg?msg=abcd' 의 URL로 리디렉트된다.



`sendmsg.html` 파일 내용:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Send Msg</title>
</head>
<body>
    <h1>Msg Sent</h1>
    <p>{{msg}}</p>  #_(2-5)_#
</body>
</html>
```

- (2-5) 사용자가 입력했던 내용이 `{{msg}}` 자리에 출력된다.

.

### - 사용자가 입력한 데이터를 메모리에 저장하여 재활용하기

예시) 사용자가 본인의 이름을 입력하면 (Faker 패키지로 랜덤 직업을 추출하여) 전생의 직업을 알려준다.

`app.py` 내용:

```python
from flask import Flask, render_template, request
from faker import Faker

app = Flask(__name__)

@app.route("/")
def index():
    return render_template('index.html')

names = {}  # 글로벌 범위의 빈 딕셔너리 생성

@app.route("/pastlife")
def pastlife():
    name1 = request.args.get('name')
    fake = Faker()
    if name1 in names:
        job = names[name1]
    else:
        job = fake.job()
        names[name1] = job
    return render_template('pastlife.html', name1=name1, job=job)
```

- `names = {}` 딕셔너리는 특정 함수의 범위에 속해있지 않기에, 복수의 `@app.route()` 에서 재활용 가능하다.
- 위의 pastlife() 함수 내 조건문은 사용자가 입력한 이름이 딕셔너리에 존재하지 않는 새로운 이름이면 Faker 패키지를 통해 랜덤 직업을 생성하여 출력하고, 이미 딕셔너리에 존재하는 이름이라면 이전에 출력했었던 직업을 다시 출력함으로써 프로그램의 신빙성을 높인다.



`index.html` 내용:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Past Life</title>
</head>
<body>
    <h1>Enter Your Name:</h1>
    <form action="/pastlife">
        <input type="text" placeholder="Name" name="name">
        <input type="submit" value="Check Pastlife">        
    </form>
</body>
</html>
```



`pastlife.html` 내용

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Past Life</title>
</head>
<body>
    <h1>{{name1}}'s Past Life</h1>
    <p>Occupation: {{job}}</p>
</body>
</html>
```

- 사용자가 'jiwon'이라는 이름을 입력했을 때, 리디렉트되는 웹페이지 URL은 'https://flask-whitejcme.c9users.io/pastlife?name=jiwon' 이 된다 (form에서 GET 방식으로 데이터를 넘겼기 때문)





===============================================================================

# Flask에서 사용되는 Jinja2 문법

`{{ ... }}`

- `app.py` 파일로부터 불러와 렌더링할 html 파일에 특정 데이터를 전달할 때, 변수명을 위 기호 안에 작성한다

`{# ... #}`

- 주석 처리

.

##### if 문

```jinja2
{% if variable = "value" %}
	<p>variable is "value"</p>
{% else %}
	<p>variable is not "value"</p>
{% endif %}
```

##### for 문

```jinja2
{% for item in some_list %}
<p>{{item}}<p>
{% endfor %}
```

.

## Jinja Template Inheritance

공통되는 html 코드는 템플릿화하여 렌더링할 여러 html파일에 재사용할 수 있다 (html코드의 변수화)

##### 세팅

- `layout.html` -> 기본 템플릿을 담을 html파일. `templates` 폴더 안에 위치해 있어야 한다.
- 외부 파일로 작성한 css 부분을 템플릿에 넣고자 한다면, `templates` 폴더와 동일한 레벨에 생성한 `static` 폴더에 `style.css` 등의 파일을 저장한다.

##### `layout.html`

- (대부분의 탬플릿 생성 개념은 http://flask.pocoo.org/docs/1.0/patterns/templateinheritance/ 참고하면 된다)

- css 외부 스타일 시트의 경우:

  - 원래 `<style>` 태그가 들어가야 할 부분 (`<head>` 부분에서 `<title>` 윗줄)에 `{% block style %}{% endblock %}` 을 쓴다

  - 템플릿을 적용할 개별 `html` 파일에서 다음과 같은 코드를 추가한다:

    ```Jinja
    {% block style %}
    <link rel="stylesheet" href="{{ url_for('static', filename='mystyle.css') }}">
    <style>
        #tmp {
            color: dodgerblue;
        }
    </style>
    {% endblock %}
    ```

- `layout.html` 의 예시

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css">
      
      {% block style %}{% endblock %}
      <title>{% block title %}{% endblock %} - Jjal</title>
      
  </head>
  <body>
      <div id="wrap">
          <div id="line1" class="container">
              <p>Line 1 Content - Same for all pages</p>
          </div>
          
          <div id="line2" class="container">
              {% block maincontent %}
              {% endblock %}
          </div>
      </div>
      
      <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
  </body>
  </html>
  ```

- 위 템플릿을 적용한 `index.html` 예시:

  ```html
  {% extends "mylayout.html" %}
  
  {% block style %}
  <link rel="stylesheet" href="{{ url_for('static', filename='mystyle.css') }}">
  <style>
      #tmp {
          color: dodgerblue;
      }
  </style>
  {% endblock %}
  
  {% block title %}Jjalbang Home{% endblock %}
  
  {% block maincontent %}
      <div class="container">
          <div class="row">
              <h1 id="tmp">짤방 검색기</h1>
              <p>(index.html 에만 삽입될 내용)</p>
          </div>
      </div>
  {% endblock %}
  ```




