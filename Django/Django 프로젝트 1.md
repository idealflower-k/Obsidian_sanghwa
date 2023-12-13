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
```
docker compose up -d
```
- 위 명령어로 2개의 컨테이너를 백그라운드로 실행시킨다.
```
docker compose exec -it python django-admin
```