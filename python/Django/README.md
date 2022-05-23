# Django 공통

### 목차

```
0. Django
1. 시작하기
2. URLS
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

