``` python
def news_view(request, topic):
	try:
		return HttpResponse(articles[topic])
	except:
		raise Http404("No such topic")
```
## 의문
Http404 객체를 raise했지만 except하는 구문은 없는 상황이지만 클라이언트는 정상적으로 404에러 페이지를 받을 수 있다.
## 이유
- *Http404* 예외는 Django 프레임 워크가 특별하게 처리하는 내장 예외 중 하나이다.
## 과정
1. 예외 캡쳐: Django는 View 함수에서 발생한 예외를 캡쳐한다. 이 예외는 일반적인 예외처리(try/except)블록이 아닌, Django의 미들웨어 시스템에 의해 처리된다.
2. Django는 자동으로 클라이언트에게 404 응답을 보낸다. 이 응답은 일반적으로 사용자 정의 404 에러 페이지로 표시된다.
3. Django는 *templates* 디렉토리에서 *404.html* 파일을 찾아서 사용자에게 보여주거나, 기본 404페이지를 표시한다.

## 미들웨어 시스템
> 요청과 응답을 처리하는 동안 중간에 개입하여 추가적인 처리를 수행하는 일련의 후크(hooks) 또는 기능들을 말한다.