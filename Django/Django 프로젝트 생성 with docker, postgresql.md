#django 
# Django-admin tool
- pip install django 로 장고 설치시 자동으로 설치된다.
# docker, docker compose
``` docker
FROM python:3

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /code

COPY requirements.txt /code/requirements.txt

COPY . /code/

EXPOSE 8000

RUN pip install -r requirements.txt
```
``` yml
version: "3"

services:

	web:

		container_name: web
		
		build: .
		command: python manage.py runserver 0.0.0.0:8000
		# command: tail -f /dev/null
		
		ports:
			- "8000:8000"
		
		volumes:
			- .:/code
		
		environment:
			- POSTGRES_NAME=postgres
			- POSTGRES_USER=postgres
			- POSTGRES_PASSWORD=postgres
		
		depends_on:
			- db

  

	db:
		container_name: db
		
		image: postgres:13
		
		pull_policy: always
		
		volumes:
			- postgres_data:/var/lib/postgresql/data/

		environment:
			- POSTGRES_DB=postgres
			- POSTGRES_USER=postgres
			- POSTGRES_PASSWORD=postgres

volumes:
	postgres_data:
```
---
## 프로젝트 생성
- 아래 명령어로 프로젝트를 생성한다.
```
docker-compose run web django-admin startproject project_name .
```
- project_name/settings.py 의 DATABASE 부분을 수정한다.
``` python
import os

DATABASES = {
	'default': {
		'ENGINE': 'django.db.backends.postgresql',
		'NAME': os.environ.get('POSTGRES_NAME'),
		'USER': os.environ.get('POSTGRES_USER'),
		'PASSWORD': os.environ.get('POSTGRES_PASSWORD'),
		'HOST': 'db',
		'PORT': 5432,
	}
}
```
- 컨테이너를 백그라운드로 실행한다.
``` zsh
docker compose up -d
```
## DB 마이그레이션
- Django 애플리케이션을 정상적으로 사용하려면 먼저 DB 마이그레이션(migration)을 실행하여 관련 테이블과 생성하고 필요한 데이터를 적재해줘야 한다.
- 현재 백그라운에서 돌아가고 있는 `web` 서비스를 대상으로 날려준다.
``` zsh
docker-compose exec web python manage.py migrate
```
- 아래 명령어로 db상태를 확인할 수 있다.
``` zsh
docker-compose exec -it db psql postgres postgres

# 명령어 이후 '\z' 입력
postgres=# \z
```
