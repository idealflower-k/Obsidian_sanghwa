#django
## Model - Template - View (MTV) 구조
### Model
- 데이터베이스에 연결하는 모델
- 
### Template
- 사용자 브라우저에 연결하는 템플릿
- 템플릿 자체는 HTML파일이 될 것이므로, 템플릿 == HTML, JS, CSS
- 브라우저가 사용자에게 표시하기 위해 정보를 직접 읽는 곳
#### {{JINJA}}
- python 파일에서  해당 템플릿으로 직접 정보를 삽입할 수 있다.
### View
- 모델과 템플릿 간의 연결인 뷰
- Views.py
- 파이썬 파일
- 파이썬 파일은 본질적으로 함수와 데코레이터 사용의 리스트이이다.
- 이를 통해 python에서 직접 해당 템플릿에 정보를 주입할 수 있다.
- 특정 URL 라우팅과 함께 작동한다. ( urls.py )
### MTV 구조를 따를 수 있게 해주는 특징
1. ORM - object-relational Mapper
2. Models
3. URLs and Views
4. Templates

