#python 
## asyncio 에 의한 단일 스레드 병렬 작업
- 함수 호출을 병렬로 처리하는 동작을 사용하기 쉽게 만들고, 직접 스레드를 제어하는 것이 아닌 **Future** 객체를 사용함으로써 자바스크립트의 Promise 개념을 도입한 것으로 평가된다.
- 새로운 병렬처리 API와 더불어 **Future** 클래스가 도입된 것이 파이썬 3.2였다.
## 비동기 I/O와 코루틴
- DB 쿼리 요청이나 네트워크 요청, 파일 I/O 등의 작업은 CPU를 거의 사용하지 않지만, 해당 작업에 대한 호출이 리턴할 때까지 스레드흐름이 멈춘 상태가 된다.
	  이러한 대기시간 동안에 해당 작업 외에 다른 작업으로의 전환을 단일 스레드에서 처리할 수 있는 기반 기술들은 이미 파이썬 내에 구비되어 있었는데, 바로 **코루틴**이다.
- 파이썬의 제너레이터는 제너레이터 함수를 호출하여 생성되는 객체로 하나 이상의 값을 반복적으로 리턴할 수 있다. 일반적인 함수는 값을 리턴한 후에 그 내부 스코프의 모든 데이터가 파괴되지만 제너레이터는 "실행을 멈춘 상태"로 기다리다 다시 호출되면 "그자리에서 이어서" 실행된다.
- CPU에 부하가 집중되는 다른 작업들과는 달리 I/O 작업은 CPU와 개별적으로 동작이 가능하다는 점에 착안하여, I/O 처리를 기다리는 역할을 코루틴에게 위임하고, 그 시간동안 다른 코루틴을 이용해서 별도의 작업을 또 비동기로 처리할 수 있다.
- 이러한 작업을 독립적으로 처리해줄 수 있는 구조 -> 코루틴, 미완료 작업에 액세스할 수 있는 방법이 Future에 의해 준비됨 -> 단일 스레드 기반 non-blocking API 출시
## asyncio 라이브러리
- 이 글에서 말하는 코루틴은 제너레이터 문법을 이용해서 메인 루틴과 흐름을 분리하여 동작하는 기존의 코루틴과는 완전히 구분될 필요가 있다.
- asyncio.coroutine()에 의해 생성되는 비동기 I/O처리를 위한 코루틴을 의미한다.
- **async, await** 는 이 비동기 코루틴을 위해 새로 추가된 문법이다. 이는 asyncio의 API를 위한 문법이며, 제너레이터를 이용한 기존의 코루틴은 이 문법을 적용하지 않는다.
- **async** 는 코루틴으로 정의하려는 함수의 **def** 앞에 붙이며, 그 의미는 **async**가 붙어서 정의된 함수는 비동기로 호출되는 코루틴이라는 뜻
- **await** 는 코루틴 내에서 다른 코루틴을 호출하고 그 결과를 받을 때 사용하며, 다른 비동기 코루틴을 호출하되, 해당 작업이 완료될 때까지 기다린다는 뜻
## 코루틴이 시작되는 방법
- **async** 지시어를 붙여서 정의한 함수(코루틴 함수)는 정확히 말하면 코루틴을 생성해주는 함수이며, 생성된 코루틴을 자동으로 실행하지는 않는다.
- 실제로 실행하기 위해서는 코루틴을 런루프에 스케줄링해야한다.
	- 파이썬 인터프리터가 코드를 실행하는 동안 코루틴이 스레드처럼 병렬로 실행되지 않는다.
	- 해당 스레드의 코드 흐름이 어디선가 **await**를 만나는 순간 멈춰서 대기하게 되는데, 이 때 스케줄링 되어 있던 다른 코루틴이 동작한다.
> 파이썬 3.7qnxjsms **asyncio.create_task()** 라는 보다 시용자 친화적인 이름의 함수가 등장했다.
- 코루틴을 스케줄링하려면 **asyncio.ensure_future()** 를 이용한다.
	- 이 함수는 코루틴 객체를 인자로 받아서 **asyncio.Task** 객체를 리턴해주는데 이 **Task** 객체는 **asyncio.Future** 의 서브 클래스이며, **concurrent.futures** 에 정의된 **Future** 클래스와 거의 동일한 API를 제공한다.
- 즉, **asyncio.ensure_future()** 는 병렬처리 모듈인 **concurrent.futures** 의 **Executor.submit()** 과 동일한 역할을 한다고 할 수 있다.
## 런루프
- 런루프는 일종의 무한루프이다.
- 특정한 이벤트나 콜이 발생하면 런루프에 해당 작업이 등록된다. 그리고 루프의 말미에 처리해야 할 함수들을 차례로 호출해 주는 것이다.
- 따라서, **ensure_future()** 함수가 실행되려면 코루틴을 걸어둘 런루프가 필요하며, 비동기 작업을 처리하기 전에는 런루프를 돌려야한다.
	  **get_event_loop()** 함수를 통해서 얻고, **run_until_\*()** 함수들을 통해서 돌릴 수 있다.
> 파이썬 3.7에서는 이러한 작업들 마저 **asyncio.run()** 함수를 사용하여 한 번에 처리할 수 있다.
> 파이썬 3.8에서는 **python -m asyncio** 로 쉘을 시작하면 쉘 자체가 런루프 내에서 돌아간다.
## 비동기 코루틴을 호출하는 방법
1. 비동기로 처리될 루틴을 코루틴으로 정의한다.
2. 런루프를 생성하고
3. 런루프 스케줄링한 다음
4. 런루프를 돌려 코루틴이 끝나기를 기다린다.
``` python
import asyncio

async def lazy_greet(msg, delay=1):
	await asyncio.sleep(delay)
	print(msg)

loop = asyncio.get_event_loop()
loop.run_until_complete(lazy_greet("hello", 3))
loop.close()
```
1. lazy_greet 코루틴은 몇 초의 딜레이를 가진 후에, 입력받은 문구를 출력하도록 구성되었다.
2. get_event_loop() 함수를 써서 런루프를 얻고
3. run_until_complete()를 이용해서 이를 돌리면서 코루틴을 넘겨준다. 이 메소드는 코루틴이나 Future 객체를 받아서 스케줄링한다. 코루틴이 전달된 경우에는 이를 내부에서 Future로 래핑하여 처리한다. 이 함수는 넘겨받은 코루틴이 실행을 끝내면 리턴한다.
## 여러 개의 비동기 작업을 스케줄링해보자
- 여러개의 코루틴을 한번에 스케줄링하면 순서대로 실행되면서 내부에서 **await** 가 등장하면 다른 코루틴으로 제어권이 넘어간다.
- 아래 예제는 여러 코루틴을 한번에 스케줄링하고 **as_complete** 를 이용해서 하나씩 그 결과를 얻어 처리하는 가장 기본적인 패턴이다.
``` python
import asyncio
import random

async def lazy_greet(msg, delay=1):
	print(msg, "will be displayed in", delay, "seconds")
	await asyncio.sleep(delay)
	return msg.upper()

async def main():
	messages = ["hello", "world", "apple", "banana", "cherry"]
	fts = [asyncio.ensure_future(lazy_greet(m, random.randint(1, 5))) for m in messages]
	for f in asyncio.as_completed(fts):
		x = await f
		print(x)

loop = asyncio.get_event_loop()
loop.run_until_complete(main())
loop.close()

# python 3.7+
asyncio.run(main())
```
1. lazy_greet() 는 지연 시간 후에 메시지를 출력한 후에 해당 메시지를 대문자로 변환해서 리턴한다.
2. 맨 먼저 런루프에 대해 main() 이 스케줄링되고 즉시 실행된다.
3. main() 내에서는 fts를 생성하면서 다섯개의 코루틴이 스케줄링 된다. 하지만 현재 스레드에서 제어권은 아직 main()에 있으므로 블록되지 않고 넘어간다.
4. for 문에서 첫번째 await 가 등장한다. 이제 main 코루틴의 실행은 여기서 잠시 멈추고 fts 내의 첫번째 코루틴이 실행된다.
5. lazy_greet('hello')에서 먼저 메시지를 출력한다. 그런 다음 await 을 만났으니 여기서 일시정지하고 스케줄링된 다음 코루틴으로 넘어간다.
6. lazy_greet('world')가 시작된다.
7. 처음 최소 1초간은 모든 코루틴이 await에서 멈춰있다.
8. fts 내 다섯 코루틴 중에서 지연시간이 가장 짧은 코루틴의 sleep이 끝난다. 그럼 해당 코루틴은 await다음 부터 진행한후 리턴한다.
9. 하나의 코루틴이 리턴하면, 이벤트 루프는 그 다음번 기다리고 있는 코루틴으로 제어권을 넘겨준다. main() 혹은 다른 lazy_greet()
10. 이런식으로 런루프부터 먼저 끝난 코루틴이 발생할 때마다 **x = await f** 가 평가 완료되고 그 결과가 하나씩 출력된다.
## Task / Future
- asyncio 모듈의 Fature는 아직 완료되지 않고 실행중일 코루틴 작업을 감싸는 클래스이며, **concurrent.future.Future** 와 거의 같은 API를 제공하고 있다.
	- result() 를 이용해서 결과를 얻거나
	- done(), cancelled()를 이용해 완료/취소여부를 확인할 수 있으며, cancel() 메소드로 취소할 수 있다.
	- add_done_callback() 을 이용해서 완료 콜백함수를 삽입할 수 있다.
- 