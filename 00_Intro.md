



# Create New Workspace on C9.io

(start with 'Blank' Template)

Open new terminal, and run the following codes 

- (copy & paste line by line)

```shell
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc
exec "$SHELL"
```

```shell
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
exec "$SHELL"
```

```shell
pyenv install 3.6.7
pyenv global 3.6.7
```

- At this point, check that `python --version` yields `Python 3.6.7`



# Create New Folder and Project

Note) A single C9 workspace will hold multiple Django projects. Thus create a new folder for each new project.

Ex) Project name : `practice`

##### Create new folder (via terminal)

- `mkdir PRACTICE`
  - for readability, the name of the main folder should be written in all-caps
  - Note that `PRACTICE` is NOT the name of the project
- `cd PRACTICE`

##### Set up Virtual Environment

- `pyenv virtualenv 3.6.7 practice-venv`
- `pyenv local practice-venv`
  - check that `(practice-venv)` has now been prepended to the terminal prompt
  - `(practice-venv)` indicates that the virtual environment has been activated
    - `cd ../` to break out of the virtual environment
    - `cd PRACTICE/` to return to the virtual environment

##### Install Django and Create New Project

- `pip install django`

- `django-admin startproject practice .`

  - At this point, `tree .` should yield:

  - ```
    .
    ├── manage.py
    └── practice
        ├── __init__.py
        ├── settings.py
        ├── urls.py
        └── wsgi.py
    
    1 directory, 5 files
    ```

- **Additional)** 

  - **Pycharm Pro: Start here** - Create new project with the 'Django' Template (base interpreter should preferably be Python 3.7+), and IDE will automatically create everything until this point.
  - `pip install ipython django_extensions`  (may be installed along with `django`)
    - add `django_extensions` to  `INSTALLED_APPS` in `settings.py`

##### Validify HTTP_HOST header

- In `settings.py` (`PRACTICE/practice/settings.py`), modify:
  - e.g): `ALLOWED_HOSTS = ['django-prac-whitejcme.c9users.io']`
      - (where `django-prac` is the name of the workspace)
  - OR, `ALLOWED_HOSTS = ['*']` also works (esp. for Pycharm Pro initiated local projects)

##### Internationalization : Adjust server time zone and language

- Deactivate server
  - Preferably, rerun server upon modifying `settings.py` or other core settings
- In `settings.py`, modify:
  - `TIME_ZONE = 'Asia/Seoul'`   (originally `'UTC'`)
  - `LANGUAGE_CODE = 'ko-kr'`  (originally `'en-us'`)

##### Run Server

- `python manage.py runserver $IP:$PORT`
  - `$IP:$PORT` is for C9 only
- Via local environment: `python manage.py runserver` and then access `http://localhost:8000/` on a webbrowser



# Create Application (within Project)

Ex) Application name: `pages`

##### Setting Up

- `python manage.py startapp pages` to create new app
- In `PRACTICE/practice/settings.py`:
  - add `'pages',` as the last item of `INSTALLED_APPS`
- In `PRACTICE/practice/urls.py`:
  - add `from pages import views` toward top of document
- Create a new `templates/` folder under `PRACTICE/pages` 
  - (this is where all `html` files will go)

##### Routing - Basic

- In `PRACTICE/pages/views.py`, add:

  - ```python
    def index(request):
        return render(request, 'index.html')
    ```

  - (where `index` is the path name)

  - `request` must always be the first argument

- In `PRACTICE/practice/urls.py`, add:

  - `path('index/', views.index),` as last item of `urlpatterns`
  - Note) Path format: `path(url_path_name, 해당 요청을 다룰 view 함수),`
    - e.g) `path('index/', views.index),`  => https://django-prac-whitejcme.c9users.io/index/
    - e.g2) `path('', views.index),` =>  https://django-prac-whitejcme.c9users.io/ (will render `index.html`)

- Create `index.html` under `PRACTICE/pages/templates/`

##### Routing - Transferring data/variables to the corresponding html file

- `PRACTICE/pages/views.py`

  - ```python
    def index(request):
        name = "Jiwon"
        msg = "Hello"
        return render(request, 'index.html', {'name': name, 'msg': msg})
    ```

  - OR

  - ```python
    def index(request):
        name = "Jiwon"
        msg = "Hello"
        context = {
            'name': name,
            'msg': msg
        }
        return render(request, 'index.html', context)
    ```

  - (transfer in python dictionary format)

##### Variable Routing (e.g cube)

- `PRACTICE/pages/views.py`

  - ```python
    def cube(request, number):
        # 사용자로부터 URL을 통해 입력 받은 값을 세제곱한다.
        result = number ** 3
        return render(request, 'cube.html', {'result': result})
    ```

- `PRACTICE/practice/urls.py`

  - add `path('cube/<int:number>', views.cube),` as last item of `urlpatterns`

- create `cube.html` under `PRACTICE/pages/templates/` 

  - ```html
    <h1>Cubed: {{result}}</h1>
    ```







# Django Conventions

- trailing comma















//