# Django 공통

### 목차

```
0. Django
1. 시작하기
2. URLS
3. VIEWS
```

### 0. Django

* 검증된 Python 언어 기반 Web framework
* 대규모 서비스에서 안정적으로 세계적 기업들에 의해 사용 됨(Instagram, Dropbox 등)

### 1. 시작하기

* 가상환경 생성 및 활성화

  ```
  ## 새로운 프로젝트의 경우
  python -m venv venv
  pip install django
  pip freeze > requirements.txt
  
  ## 기존 프로젝트를 Clone한 경우
  python -m venv venv
  pip install -r requirements.txt
  ```

* 프로젝트 생성

  ```
  django-admin startproject [생성할 프로젝트 이름] [경로]
  	ex) django-admin startproject firstpjt .
  ```

* 서버 실행

  ```
  python manage.py runserver
  ```

* 앱 생성

  ```
  python manage.py startapp [생성할 앱 이름(복수형)]
  ```

  ```
  ## pjt/settings.py
  INSTALLED_APPS = [
  	# Local apps
  	'[내가 만든 앱 이름]',
  	
  	# Third party apps : 기능을 위해 pip를 통해 설치한 앱들
  	...
  	
  	# Django apps : 장고의 기존 앱들
  	...
  ]
  ```

* 관리자 계정 생성

### 2. URLS

* HTTP요청(request)을 알맞은 view로 전달

* 프로젝트 urls.py 설정

  ```
  ## pjt/urls.py
  from django.contrib import admin
  from django.urls import path, include
  
  urlpatterns = [
  	path('admin/', admin.site.urls),
  	path('[보내고싶은 경로]/', inclued('[내가 만든 앱.urls]')),
  ]
  ```

* 앱 urls.py 설정

  ```
  ### app/urls.py : 앱의 기본설정에 없기 때문에 수작업으로 생성해야 함
  from django.urls import path
  from . import views
  
  urlpatterns = [
  	path('', views.index),
  ]
  ```

* Variable Routing

  * URL 주소를 변수로 사용 : URL의 일부를 변수로 지정하여 view함수의 인자로 넘김

  ```
  ## urls.py
  path('/accounts/user/<int:user_pk>', views.user_detail)
  	* accounts/user/1
  	* accounts/user/2
  ```

  ```
  views.py
  def user_detail(request, user_pk):			# user_pk = variable routing으로 받아온 값
  ```

* Namespace & Naming URL patterns

  * Naming URL patterns : 링크에 url을 직접 작성하지 않고, path()의 name인자를 정의해 사용

  ```
  ## urls.py
  path('index/', views.index, name='index')
  ```

  ```
  ## app/templates/app/index.html
  <a href="{% url 'index' %}">메인페이지</a>
  ```

  * Namespace : 서로 다른 앱에서 동일한 name의 path를 사용하는 경우 정확한 경로를 찾기위해 사용

  ```
  ## urls.py
  app_name = 'accounts'
  urlpatterns = [
  	path('index/', views.index, name='index')
  ]
  ```

  ```
  ## app/templates/app/index.html
  <a href="{% url 'accounts:index' %}">메인페이지</a>
  ```

  

### 3. VIEWS

* HTTP요청을 수신하고 HTTP응답을 반환하는 함수 작성
* Model을 통해 요청에 맞는 데이터에 접근

























































