//



## CRUD URL (RESTful API)

##### READ

`/articles` 			=> list

`/articles/1`		=> detail

##### CREATE

`/articles/new` 		=> write new

`/articles/create`	=> save to DB


##### UPDATE

`/articles/1/edit` 	=> make edits

`/articles/1/update`	=> update & save to DB

##### DELETE

`/articles/1/delete` => delete (apply to DB)





## Sub-URL

Create a separate `urls.py` file for each individual app to avoid redundancy in URL paths



##### main `urls.py` (same level as `settings.py`)

```python
from django.contrib import admin
from django.urls import path, include
from articles import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
]
```



##### sub `urls.py` (in `articles` app)

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index),  # https://~~/articles/
]
```







## Uploading/Resizing Images

##### Package Installation

- `pip install django-imagekit` 
  - add `imagekit` to settings.py INSTALLED_APPS
  - to resize images
- `pip install pillow`
  - to use `models.ImageField()` in models.py





## Messages Framework

=> Displaying a one-time notification message to the user

##### Package Installation

- add `MESSAGE_STORAGE = 'django.contrib.messages.storage.session.SessionStorage'` to bottom of settings.py

##### html file to render

```jinja2
{% if messages %}
    <div class="alert alert-warning alert-dismissible fade show" role="alert">
        {% for message in messages %}
            {{ message }}
        {% endfor %}
      <button type="button" class="close" data-dismiss="alert" aria-label="Close">
        <span aria-hidden="true">&times;</span>
      </button>
    </div>
{% endif %}
```

##### views.py

```python
def index(request):
    ...
    messages.success(request, 'Message Content')
    return render(request, '~~.html')
```







## django-debug-toolbar 설치하기

1. `pip install django-debug-toolbar`

2. `settings.py`에 다음의 코드 추가하기:

   1. INSTALLED_APPS에 `debug_toolbar` 추가 ('django.contrib.staticfiles' 바로 다음에)
   2. MIDDLEWARE에 `debug_toolbar.middleware.DebugToolbarMiddleware` 추가
   3. 맨 밑에 `INTERNAL_IPS = ('127.0.0.1')` 추가

3. 메인 `urls.py`에 다음의 코드 추가하기:

   1. 맨 위에 `from django.conf import settings`, `from django.urls import path, include` 추가

   2. 맨 밑에 추가:

      ```python
      if settings.DEBUG:
          import debug_toolbar
          urlpatterns = [
              path('__debug__/', include(debug_toolbar.urls)),
          ] + urlpatterns
      ```





## Django REST API

##### Package Installation

- `pip install djangorestframework`
  - add `rest_framework` to settings.py INSTALLED_APPS
- `pip install django-rest-swagger`
  - add `rest_framework_swagger` to settings.py INSTALLED_APPS







## Heroku Deploy



`pip install django-heroku`

- settings.py 'import os' 밑에 `import django_heroku` 추가

- append `django_heroku.settings(locals())` to bottom of settings.py



add `STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')` below MEDIA_ROOT in settings.py



`pip install gunicorn`



under main project directory, create `runtime.txt`

- ```
  python-3.6.7
  ```

- (in same directory where manage.py is located)

- python --version으로 파이썬 버전 확인 



pip freeze > requirements.txt



under main project directory, create `Procfile`

- ```
  web: gunicorn instagram.wsgi
  ```

- where `instagram` is the name of project (name of directory where settings.py file is saved)



c9 terminal => `heroku login` and enter email + password

- heroku create [unique_name]
  - (if [unique_name] is omitted, heroku will automatically create a globally unique name)



git add .

git commit -m "heroku"

git push heroku master



heroku run python manage.py migrate

heroku run python manage.py createsuperuser



heroku domains:add [domainURL]









## Easy Package Installation with `pip freeze`

 `pip install` 로 설치한 모든 패키지는 (.gitignore에 포함된) venv에 설치되기 때문에, git clone을 할 때마다 리셋된다. 이 경우에 `pip freeze` 명령어를 통해 기존에 설치해두었던 패키지들을 간편하게 복구할 수 있다.

- `pip freeze` => 기존에 설치하였던 패키지들의 목록 및 구체적인 버전을 알려준다.
- `pip freeze > requirements.txt` => 위의 목록이 프로젝트 디렉토리 내에 텍스트파일로 저장된다.
- `pip install -r requirements.txt` => 텍스트파일의 모든 패키지들을 한줄 한줄 설치한다.




//