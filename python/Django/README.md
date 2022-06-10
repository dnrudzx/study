# Django 공통

### 목차

```
0. Django
1. 시작하기
2. URLS
3. VIEWS
4. Model
5. Admin
6. Static files
7. File upload
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

### 4. Model

* 웹 애플리케이션의 데이터를 구조화하고 조작하기 위한 도구

* 저장된 데이터베이스의 구조(Schema)

* Django는 모델을 통해 데이터에 접근하고 관리

* ORM(Object Relational Mapping)사용 : 호환되지 않는 유형의 시스템(Django - SQL)간 데이터를 변환

  ```
  형태 : [Class Name].objects.[QuerySet API]
  	Class Name : 모델에 작성된 클래스 이름
  	objects : django모델에 데이터베이스 query작업이 제공되는 인터페이스(objects = default)
  	QuerySet API : DB로 부터 전달받은 객체 목록
  ```

* Migrations : django가 model에 생긴 변화를 반영하는 방법

  ```
  makemigrations : 모델의 변경에 대한 새로운 설계도(migration)을 작성
  migrate : 설계도(migration)을 DB에 반영
  sqlmigrate : migration이 DB에 어떻게 반영되는지 SQL문으로 확인
  showmigrations : 프로젝트 전체 migration상태를 확인
  
  $ python manage.py [mirgation조작어]
  ```

* Django Model Fields : https://docs.djangoproject.com/en/4.0/ref/models/fields/#field-types

* CRUD : 예시를 위해 Class Name은 Article로 사용

  ```
  Create
  article = Article()
  article.title = '제목'
  article.save()
  ```

  ```
  Read
  전체 조회 : Article.objects.all()							QuerySet반환
  하나 조회 : Article.objects.get(pk=100)						모델과 동일한 형태의 객체 반환
  	객체가 없는 경우 : DoesNotExist 예외 출력
  	객체가 여러개인 경우 : MultipleObjectReturned 예외 출력
  조건 조회 : Article.objects.filter(title='first')			QuerySet반환
  	Django Filter : 
  		https://docs.djangoproject.com/en/4.0/ref/models/querysets/#field-lookups-1
  이외에도 exclude 등 다른것들도 있음
  ```

  ```
  Update : 인스턴스 객체의 변수값을 변경 후 저장
  article = Article.objects.get(pk=1)
  article.title = '수정된 제목'
  article.save()
  ```

  ```
  Delete : 객체 및 QuerySet의 모든 행에 대해 SQL삭제 쿼리 실행
  article2 = Article.objects.get(pk=1)
  article.delete()
  article = Article.objects.all()
  article.delete()
  ```

* 예시

  ```
  ## app/model.py
  from django.db import models
  
  class Article(models.Model):
  	title = models.CharField(max_length=10)					# 짧은 문자열 필드
  	content = models.TextField()							# 긴 문자열 필드
  	create_at = models.DateTimeField(auto_now_add=True)		# 시간 저장(생성시 자동 저장)
  	update_at = models.DateTimeField(auto_now=True)			# 시간 저장(변결시 자동 저장)
  	
  	def __str__(self):										# 각각의 object를 반환시
  		return self.title									# 어떻게 표현할지를 정의
  ```

### 5. Admin

* Django가 제공하는 관리자 페이지

* Model Class를 admin.py에 등록하고 관리

* 사용방법

  ```
  admin 계정 생성
  $ python manage.py createsuperuser
  ```

  ```
  모델 등록
  ## app/admin.py
  from django.contrib import admin
  from .models import [Class Name]							# 모델에 정의한 클래스
  
  admin.site.register([Class Name])		# 모델에 __str__로 정의한 경우 원하는 형태로 출력
  ```

  ```
  모델 커스텀
  ## app/admin.py
  from django.contrib import admin
  from .models import [Class Name]
  
  class [Class Name]Admin(admin.ModelAdmin):			# 사실 클래스 이름은 마음대로 해도 됨
  	list_display = ('pk','title','content',)
  
  admin.site.register([Class Name], [Class Name]Admin)
  													# 파라미터로 모델, 위에서 정의한 클래스
  ```

* 기타 모델 옵션 : https://docs.djangoproject.com/en/4.0/ref/contrib/admin/#modeladmin-options

### 6. Static files

* 정적 파일

* 응답시 별도의 처리 업이 파일 내용을 그대로 보여주는 파일(img, js, css 등)

* 모든 정적파일을 한곳에 저장 : 배포 환경에서 모든 정적 파일을 다른 웹 서버가 제공 가능

  ```
  ## settings.py
  STATIC_ROOT = BASE_DIR / 'staticfiles'
  ```

  ```
  $ python manage.py collectstatic
  ```

  ```
  settings.py에서 설정한 경로에 css/fonts/img/js 등이 저장 됨
  ```

* 개발 단계에서 실제 정적 파일들이 저장 : 두 가지 경로를 탐색

  ```
  ## settings.py
  STATIC_URL = '/static/'				# app/static/경로
  									# static파일간의 충돌 방지 : app/static/app/fileName
  
  STATICFILES_DIRS = [				# 아래에 설정된 모든 경로를 공통적으로 탐색
  	BASE_DIR / 'static',
  ]
  ```

  ```
  ## app/templates/app/???.html
  {% load static %}
  
  <img src="{% static 'app/fileName.jpg' %}"			# STATIC_URL에서 가져온 경로
  <img src="{% static 'images/fileName.jpg' %}"		# STATICFILES_DIRS에서 가져온 경로
  ```

### 7. File upload

* 사용자가 웹에서 업로드하는 정적 파일

* FileField를 상속받는 서브 클래스들(본문에서는 ImageField 사용)

  * upload_to : 업로드 디렉토리와 파일 이름을 설정(문자열을 통해 경로 지정 / 함수 호출)

    ```
    ## models.py
    def myModel_file_path(instance, filename):
    	# MEDIA_ROOT/ 뒤에 설정한 경로로 업로드
    	return f'user_{instance.user.pk}/{filename}'
    	
    class MyModel(models.Model):
    	# 문자열을 통해 경로 지정 / %Y, %m, %d 등을 통해 시간 정보 넣기
    	upload = models.FileField(upload_to='uploads/%Y/%m/%d')
    	# 함수 호출을 사용해 경로 지정(반드시 instance, filename 인자 사용)
    	upload = models.FileField(upload_to=myModel_file_path)
    ```

  * ImageField : 이미지 업로드에 사용 / 사용시 Pillow 라이브러리 필요

* 세팅(ImageField 사용이니까 Pillow 설치)

  ```
  $ pip install Pillow
  ```

  ```
  ## settings.py
  # 사용자가 업로드 한 파일들을 보관할 절대 경로
  # 반드시 STATIC_ROOT와 다른 경로로 지정
  MEDIA_ROOT = BASE_DIR / 'media'
  # 업로드 된 파일의 주소(URL)을 만들어 주는 역할(개발자 도구에 표시되는 주소)
  MEDIA_URL = '/media/'
  ```

  ```
  ## pjt/urls.py
  from django.contrib import admin
  from django.urls import path, include
  from django.conf import settings										# 추가
  from django.conf.urls.static import static								# 추가
  
  urlpatterns = [
  	path('admin/', admin.site.urls),
  	path('[보내고싶은 경로]/', inclued('[내가 만든 앱.urls]')),
  ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)		# 추가
  ```

  ```
  ## app/models.py
  class MyModel(models.Model):
  	title = models.CharField(max_length=20)
  	content = models.TextField()
  	# 이미지 업로드 / 경로 : MEDIA_ROOT/images
  	# blank=True : 업로드 안하는 것을 허용(기본값 = False)
  	image = models.ImageField(blank=True, upload_to='images/')
  ```

* Create

  ```
  ## app/views.py
  def create(request):
  	if request.method == 'POST':
  		form = ArticleForm(request.POST, request.FILES)
  		if form.is_valid():
  			article.save()
  			return redirect('articles:detail', article.pk)
  	else:
  		form = ArticleForm()
  	context = {
  		'form': form,
  	}
  	return render(request, 'articles/create.html', context)
  ```

  ```
  ## app/templates/app/create.html
  <form action="{% url 'articles:create' %}" method="POST" enctype="multipart/form-data">
  	{% csrf_token %}
  	{{ form.as_p }}
  </form>
  ```

  * multipart/form-data : 파일/이미지 업로드시 반드시 사용

* Read

  ```
  ## app/templates/app/detail.html
  {% extends 'base.html' %}
  {% block content %}
  	<!-- Model.Field.url 을 통해 경로를 가져올 수 있다. -->
  	<img src="{{ article.image.url }}" alt="{{ article.image }}">
  {% endblock %}
  ```

  



















































