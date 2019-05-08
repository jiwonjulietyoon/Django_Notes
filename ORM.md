### *) Relevant files

- `models.py`
- `db.sqlite3`





# Create DB Table (Model) 

### In `models.py`:

```python
class Article(models.Model):
    title = models.TextField()
    content = models.TextField()
    def __repr__(self):
        return f"제목: {self.title}, 내용: {self.content}"
    def __str__(self):
        return f"제목: {self.title}, 내용: {self.content}"
```

- Name of table is automatically created as `Article` => same as class name
- column `id` is automatically created (if not already included in the above class definition)
- `def __repr__(self)`: return value is printed when attempting to access the data record itself
- `def __str__(self)`: return value is printed when attempting to print the data record



# Add Model to DB (Migration)

##### 1. `python manage.py makemigrations`

- Creates new migrations based on the changes made to models (in `models.py`)
- Will create files such as  `DB/articles/migrations/0001_initial.py`

##### 2. `python manage.py migrate`

- Apply (or unapply) migrations
- Will add the above table to `db.sqlite3`

##### +) `python manage.py sqlmigrate [e.g articles 0001]`

- Display the SQL statements for the specified migration



# Manipulate Data in DB (CRUD)

Example) Model name: `Article` ; Columns: `id`, `title`, `content`

##### Run via:

- Python Shell (`python manage.py shell`)
  - `exit()` to exit from shell
  - After modifying `models.py`, exit and reenter Python Shell to apply changes made
- `views.py`

##### Prerequisites

- **[Python Shell]** `from articles.models import Article`
- **[`views.py`]** `from .models import Article`

##### Alternatively, with `django_extensions` and `ipython`:

- `python manage.py dbshell`  (instead of `sqlite3 db.sqlite3`)
- `python manage.py shell`  - console format will have changed
  - e.g) `In [1]: from articles.models import Article` 
  - syntax highlighting available => thanks to django_extensions and ipython
  - Modify DB via commands such as `article = Article(title="Title Test", content="Content Test")`, `article.save()` , etc.
  - `exit()` to exit
- `python manage.py shell_plus` 
  - automatically imports `from articles.models import Article` etc 

### CREATE

- Option 1) `a = Article(title="Happy", content="Hacking")`
  `a.save()`
  - where `save()` is equivalent to making commits
- Option 2) `Article.objects.create(title="Happy", content="Hacking")`
  - one-liner

### READ

##### Read All

- `Article.objects.all()`
  - Result: `<QuerySet [<Article: Article object (1)>]>`
  - Uses)
    - `for a in Article.objects.all():`
      ​	`a`     ==> will print return value of `def __repr__(self)`
      ​	`print(a)`   ==> will print return value of `def __str__(self)`

##### Read One (first one in list)

- `Article.objects.first()`
  - Result: `<Article: Article object (1)>`
- `Article.objects.first().title`
  - Result: `'Happy'`

##### Read Specific Data Records: `get` or `filter`

- `Article.objects.filter(title="Happy").all()`
  - Result: `<QuerySet [제목: Happy, 내용: Hacking]>`
  - Note) `None` is returned when no data records match filter conditions
- `Article.objects.get(title="Happy")`  <==> `Article.objects.filter(title="Happy").first()`
  - Result: `제목: Happy, 내용: Hacking`
- `Article.objects.get(pk=1)`  <==>  ` Article.objects.get(id=1)`
  - `pk`: primary key
  - `id`: column name
- `Article.objects.filter(content="Hacking").count()`

##### Order Data Records

- `Article.objects.order_by('id').all()` : (default) ascending order
- `Article.objects.order_by('-id').all()` : descending order
  - `Article.objects.all().order_by('-id')` also works

##### Further manipulating query results (python List format)

- `len(Article.objects.all())` OR `Article.objects.all().count()` to count all data records in model
- Indexing, Slicing also works: 
  - `Article.objects.all()[0]`
  - `Article.objects.all()[0:2]`
  - `Article.objects.all()[::-1]`

### UPDATE

- `a = Article.objects.get(id=1)`
  `a.content = "Thursday"`
  `a.save()`

### DELETE

- `a = Article.objects.get(id=2)`
  `a.delete()`
  - no need to "commit" separately to DB

*) https://docs.djangoproject.com/en/2.1/topics/db/queries/ for further queries





# Admin Page

##### Register Model in `admin.py`

Ex) Model to register: `Job`

```python
from django.contrib import admin
from .models import Job             #(_1_)

class JobAdmin(admin.ModelAdmin):   #(_2_)
    list_display = ('name', 'job')  #(_2-1_)

# Register your models here.
admin.site.register(Job, JobAdmin)  #(_3_)
```

- 1) `.models` refers to the `models.py` file located in the same directory as the above `admin.py` file
- 2) Optional
  - Designate which fields to display on the Admin Page list
    - Each item in #2-1) becomes a column
  - If `list_display` is not set, the Admin Page will display a single column of the `__str__()` representation of each object
  - 2-1) Must be in tuple or list format, e.g) `('name', )`
- 3) If `class JobAdmin` is not defined, replace with: `admin.site.register(Job)`

##### Create Admin Account

- `python manage.py createsuperuser`

##### Access Admin Page

- e.g) https://django-prac-whitejcme.c9users.io/admin/
- View all registered models and data records (CRUD via GUI)



















