# template 등록 순서
## 1. Setup the Django App
1. manage.py startapp 명령어 사용
2. urls.py, views.py 파일 생성
3. view를 App URL에 매핑하고 App URL을 프로젝트 URL에 매핑한다. (use include)

## 2. Run the migrate command
1. python manage.py migrate
2. settings.py 내부의 설치된 앱을 살펴보고, 필요한 데이터베이스 테이블을 생성한다.
## 3. register the AppConfig class to the INSTALLED_APPS inside of settings.py
1. 자동으로 생성된 apps.py 파일 확인
2. settings.py 내부의 INSTALLED_APPS에 AppConfig 클래스 등록
- 앱 내부의 템플릿 디렉토리를 인식하도록 만드는 과정
## 4. register the app and any database changes with Django by running
1. python manage.py makemigrations myapp
## 5. Run python manage.py migrate again to create the model tables in our database
- 데이터베이스에 모델 테이블 생성
## 6. Create a templates dir inside your app dir
- my_site
	- my_app
		- templates
			- my_app
				- example.html
