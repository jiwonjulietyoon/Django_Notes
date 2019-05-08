



























---------------------

---------------------------------

-------------------------------------

Simple Version

--------------------------------

---------------------------------------

----------------------------------





# Setting Up

- Create the following directories in the target "application" folder (same level as `migrations`)

  ```
  templates/
  	base.html
  	(any other HTML files)
  static/
  	css/
  		style.css
  		(any other css files)
  	img/
  		(any image files to include)
  ```

- `base.html` is the template HTML file



# Create Base Template: `base.html`

```html
{% load static %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="(#BOOTSTRAP CSS LINK GOES HERE#)">
    <link rel="stylesheet" href="{% static 'css/style.css' %}">
    <link rel="icon" type="image/png" sizes="96x96" href="{% static 'img/favicon.png'%}">
    <title>{% block title %}{% endblock %}</title>
</head>
<body>     
    {% block body %}
    {% endblock %}
    <script src="(#BOOTSTRAP JS LINK GOES HERE#)"></script>
</body>
</html>
```

- `{% load static %}` : first line of document
- `{% static 'css/style.css' %}` : refer to css files in `css/`
- `{% static 'img/image1.jpg' %}` : refer to image files in `img/`
- Note) When inserting image links in `css/style.css`, use relative file paths - e.g) `../img/image1.jpg`



# Extend Base Template: other html files

```html
{% extends 'base.html' %}

{% load static %}

{% block title %}MyPage{% endblock %}

{% block body %}
    <header>
        <h1>Welcome!</h1>
    </header>
	<img src="{% static 'img/image1.jpg'%}" alt="img1">
{% endblock %}
```

- `{% extends 'base.html' %}` : first line of document
- `{% load static %}` : include when linking any image files located within `static/`







