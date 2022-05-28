참고 : https://django-bootstrap-v5.readthedocs.io/en/latest/

```
설치
$ pip install django-bootstrap-v5
```

```
## settings.py
...
INSTALLED_APPS = [
	...
	'bootstrap5',
	...
]
```

```
## template/base.html
{% load bootstrap5 %}

<!DOCTYPE html>
<html>
	<head>
		...
		{% bootstrap_css %}
		...
	</head>
	<body>
		...
		{% bootstrap_javascript %}
	</body>
</html>
```

```
## app/templates/app/???.html

{% extends 'base.html' %}
{% load bootstrap5 %}
```



