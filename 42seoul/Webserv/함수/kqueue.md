# [참고 man페이지](https://man.freebsd.org/cgi/man.cgi?kqueue)
## [**NAME**](https://man.freebsd.org/cgi/man.cgi?kqueue#end)
       kqueue, kevent -- kernel event notification mechanism
## [**LIBRARY**](https://man.freebsd.org/cgi/man.cgi?kqueue#end)
       Standard C Library (libc, -lc)
## [**SYNOPSIS**](https://man.freebsd.org/cgi/man.cgi?kqueue#end)
```c
#include <sys/event.h>

int kqueue(void);

int kevent(int kq, const struct kevent *changelist, int nchanges, struct kevent *eventlist, int nevents, const struct timespec *timeout);

EV_SET(kev, ident, filter, flags, fflags, data, udata);
```
## [**DESCRIPTION**](https://man.freebsd.org/cgi/man.cgi?kqueue#end)
```
kqueue() 시스템 호출은 필터라고 불리는 작은 커널 코드 조각의 결과를 기반으로 이벤트가 발생하거나 조건이 충족될 때 사용자에게 알릴 수 있는 일반적인 방법을 제공합니다. kevent는 (ident, filter) 쌍에 의해 식별되며 kqueue당 고유한 kevent만 있을 수 있습니다.

필터는 kevent가 등록될 때 미리 존재하는 조건이 있는지 여부를 감지하도록 초기 실행되며, 이벤트를 필터에 전달하여 평가될 때마다 실행됩니다. 필터가 조건을 보고해야 한다고 판단하면 kevent는 사용자가 검색할 수 있도록 kqueue에 배치됩니다.

사용자가 kqueue에서 kevent를 검색하려고 시도할 때도 필터가 실행됩니다. 필터가 이벤트를 트리거한 조건이 더 이상 유지되지 않는다고 나타내면 kevent는 kqueue에서 제거되고 반환되지 않습니다.

필터를 트리거하는 여러 이벤트는 여러 kevent를 생성하지 않습니다. 대신, 필터는 이벤트를 단일 struct kevent로 집계합니다. 파일 디스크립터에서 close()를 호출하면 해당 디스크립터를 참조하는 모든 kevent가 제거됩니다.

kqueue() 시스템 호출은 새로운 커널 이벤트 큐를 생성하고 디스크립터를 반환합니다. 이 큐는 fork(2)를 사용하여 생성된 자식에게 상속되지 않습니다. 그러나 RFFDG 플래그 없이 rfork(2)를 호출하면 디스크립터 테이블이 공유되어 두 프로세스 간에 kqueue를 공유할 수 있습니다.

kevent() 시스템 호출은 큐에 이벤트를 등록하고 사용자에게 대기 중인 이벤트를 반환하는 데 사용됩니다. changelist 인수는 <sys/event.h>에서 정의된 kevent 구조체 배열에 대한 포인터입니다. changelist에 포함된 모든 변경 사항은 대기 중인 이벤트가 큐에서 읽히기 전에 적용됩니다. nchanges 인수는 changelist의 크기를 지정합니다. eventlist 인수는 kevent 구조체 배열에 대한 포인터이며, nevents 인수는 eventlist의 크기를 결정합니다. nevents가 0이면 select(2)와 달리 kevent()는 즉시 반환됩니다. timeout이 NULL이 아닌 포인터인 경우 이벤트를 대기할 최대 간격을 지정하며, 이는 struct timespec으로 해석됩니다. timeout이 NULL 포인터인 경우 kevent()는 무한정 대기합니다. 폴링을 수행하려면 timeout 인수를 0으로 설정된 timespec 구조체를 가리키도록 설정해야 합니다. changelist와 eventlist에는 동일한 배열을 사용할 수 있습니다.

EV_SET() 매크로는 kevent 구조체를 초기화하기 쉽도록 제공됩니다.
```
### The kevent structure defined
```c
struct kevent {
	uintptr_t    ident;        // identifier for this event
	short        filter;       // filter for event
	u_short      flags;        // action flags for kqueue
	u_int        fflags;       // diltter flag value
	int64_t      data;         // filter data value
	void         *udata;       // opaque user data identifier
	uint64_t     ext[4];      // extensions
}
```
#### The fields of struct kevent
- **ident** : 이 이벤트를 식별하는데 사용되는 값, 정확한 해석은 연결된 필터에 의해 결정되지만 대부분 파일 디스크립터일 것이다.

- **filter** : 이 이벤트를 처리하는 데 사용되는 커널 필터를 식별한다. 미리 정의된 시스템 필터에 대한 설명은 아래에 나와 있다.

- **flags** : 이벤트에 대해 수행할 작업을 지정하는 플래그

- **fflags** : 필터별로 다른 플래그

- **data** : 필터별로 다른 데이터 값

- **udata** : 커널을 통과한 변경되지 않은 사용자의 정의 값

- **ext** : 커널과 전달되는 확장 데이터이다. ext[0] 및 ext[1] 멤버는 필터에 의해 정의 된다. 필터가 이를 사용하지 않으면 멤버는 변경되지 않은 상태로 복사된다. ext[2] 및 ext[3] 멤버는 항상 커널을 통과하여 응용 프로그램에 추가 컨텍스트를 제공하게 된다.
#### The flags field can contain the following values
- **EV_ADD** : 이벤트를 kqueue에 추가합니다. 기존 이벤트를 다시 추가하면 원래 이벤트의 매개변수를 수정하며 중복 항목을 만들지 않습니다. 이벤트를 추가하면 자동으로 활성화됩니다. 단, EV_DISABLE 플래그로 재정의되지 않는 한입니다.

- **EV_ENABLE** : 이 이벤트가 트리거될 경우 kevent()가 해당 이벤트를 반환할 수 있도록 허용합니다.
    
- **EV_DISABLE** : 이벤트를 비활성화하여 kevent()가 이를 반환하지 않도록 합니다. 필터 자체는 비활성화되지 않습니다.
    
- **EV_DISPATCH** : 이벤트 소스를 이벤트 전달 후 즉시 비활성화합니다. EV_DISABLE 참조.
    
- **EV_DELETE** : 이벤트를 kqueue에서 제거합니다. 파일 디스크립터에 연결된 이벤트는 디스크립터가 마지막으로 닫힐 때 자동으로 삭제됩니다.
    
- **EV_RECEIPT** : 이 플래그는 대기 중인 이벤트를 비우지 않고 kqueue에 대량 변경을 수행하는 데 유용합니다. 입력으로 전달될 때 EV_ERROR가 항상 반환되도록 강제합니다. 필터가 성공적으로 추가되면 데이터 필드가 0이 됩니다. 이 플래그가 발견되고 eventlist에 남은 공간이 없는 경우 이후 변경 사항은 처리되지 않습니다.
    
- **EV_ONESHOT** : 이벤트가 필터가 트리거되는 첫 번째 발생만 반환하도록 만듭니다. 사용자가 kqueue에서 이벤트를 검색한 후에는 삭제됩니다.
    
- **EV_CLEAR** : 이벤트가 사용자에 의해 검색된 후 상태가 재설정됩니다. 현재 상태가 아닌 상태 전환을 보고하는 필터에 유용합니다. 일부 필터는 내부적으로 이 플래그를 자동으로 설정할 수 있습니다.
    
- **EV_EOF** : 필터가 필터별 EOF 조건을 나타내도록 이 플래그를 설정할 수 있습니다.
    
- **EV_ERROR** : "RETURN VALUES" 아래 참조.

>미리 정의된 시스템 필터는 아래에 나열되어 있으며, kevent 구조체의 fflags와 data 필드를 통해 필터에 인수를 전달할 수 있습니다.

- **EVFILT_READ** : 이 필터는 식별자로 디스크립터를 사용하며 데이터를 읽을 수 있는 경우에 반환합니다. 디스크립터 유형에 따라 필터의 동작이 약간 다릅니다.

- 소켓(Socket):
    - listen(2)에 전달된 이전 소켓은 대기 중인 들어오는 연결이 있을 때 반환합니다. data에는 대기열 크기가 포함됩니다.
    - 다른 소켓 디스크립터는 소켓 버퍼의 SO_RCVLOWAT 값에 따라 읽을 데이터가 있는 경우 반환됩니다. fflags에서 NOTE_LOWAT 플래그를 설정하고 data에 새로운 낮은 수위 표시를 지정하여 필터를 추가할 때 이를 재정의할 수 있습니다. 반환될 때 data에는 읽을 수 있는 프로토콜 데이터의 바이트 수가 포함됩니다.
    - 소켓의 읽기 방향이 종료된 경우, 필터는 flags에도 EV_EOF를 설정하고 fflags에 소켓 오류 (있는 경우)를 반환합니다. 여전히 소켓 버퍼에 대기 중인 데이터가 있는 상태에서 연결이 끊겼음을 나타내는 EOF가 반환될 수 있습니다.
- Vnodes:
    - 파일 포인터가 파일 끝에 있지 않은 경우 반환합니다. data에는 현재 위치에서 파일 끝까지의 오프셋이 포함되며 음수가 될 수 있습니다.
    - 이 동작은 일반 파일의 경우에도 무조건적으로 읽기 이벤트가 트리거되는 poll(2)와 다릅니다. 이 이벤트는 NOTE_FILE_POLL 플래그를 fflags에 설정하여 무조건적으로 트리거할 수 있습니다.
- Fifos, Pipes:
    - 읽을 데이터가 있는 경우 반환합니다. data에는 사용 가능한 바이트 수가 포함됩니다.
    - 마지막 작성자가 연결을 끊으면 필터는 flags에 EV_EOF를 설정합니다. 이것은 새로운 작성자가 연결되면 필터가 데이터가 반환되기 전에 대기를 다시 시작할 때 필터에서 지워집니다.
- BPF 장치:
    - BPF 버퍼가 가득 찬 경우, BPF 타임아웃이 만료된 경우 또는 BPF에 "즉시 모드"가 활성화되고 읽을 데이터가 있는 경우 반환합니다. data에는 사용 가능한 바이트 수가 포함됩니다.
- Eventfds:
    - 카운터가 0보다 큰 경우 반환합니다. data에는 카운터 값이 포함되며 uint64_t로 캐스트해야 합니다.
- Kqueues:
    - 대기 중인 이벤트가 큐에 있는 경우 반환합니다. data에는 사용 가능한 이벤트 수가 포함됩니다.

- **EVFILT_WRITE** : 이 필터는 식별자로 디스크립터를 사용하며 디스크립터에 쓸 수 있는 경우 반환합니다. 소켓, 파이프 및 FIFO의 경우 data에는 쓰기 버퍼에 남아 있는 공간 양이 포함됩니다. 리더가 연결을 해제하면 필터는 EV_EOF를 설정하고 fifo의 경우 새로운 리더가 연결될 때 이를 지웁니다. 이 필터는 vnodes 또는 BPF 장치에 대해 지원되지 않습니다.

- 소켓(Socket), 파이프, FIFO:
    - 데이터를 쓸 수 있는 경우 반환합니다. 소켓, 파이프 및 FIFO의 경우 data에는 쓰기 버퍼에 남아 있는 공간 양이 포함됩니다. 리더가 연결을 해제하면 필터는 EV_EOF를 설정하며, fifo의 경우 새로운 리더가 연결될 때 이것이 지워집니다. 소켓의 경우 저수준 마크와 소켓 오류 처리는 EVFILT_READ 경우와 동일합니다.
- Eventfds:
    - 카운터에 블로킹 없이 추가할 수 있는 최대값이 data에 포함됩니다.

- **EVFILT_EMPTY** : 이 필터는 식별자로 디스크립터를 사용하며 쓰기 버퍼에 남아 있는 데이터가 없을 때 반환합니다.

- **EVFILT_AIO** : 이 필터의 이벤트는 직접 kevent()에 등록되지 않고 aio_read()와 같은 비동기 I/O 시스템 호출을 통해 예약될 때 비동기 I/O 요청의 aio_sigevent 멤버를 통해 등록됩니다. 이 필터는 aio_error()와 동일한 조건에서 반환됩니다. 이 필터에 대한 자세한 내용은 sigevent(3) 및 aio(4)를 참조하십시오.

- **EVFILT_VNODE** : 이 필터는 파일 디스크립터를 식별자로 사용하며 fflags에 모니터링할 이벤트를 지정하고 요청된 하나 이상의 이벤트가 디스크립터에서 발생할 때 반환합니다. 모니터링할 수 있는 이벤트는 다음과 같습니다:

- NOTE_ATTRIB: 디스크립터로 참조된 파일의 속성이 변경되었습니다.
- NOTE_CLOSE: 모니터링된 파일을 참조하는 파일 디스크립터가 닫혔습니다. 닫힌 파일 디스크립터는 쓰기 액세스 권한이 없었습니다.
- NOTE_CLOSE_WRITE: 모니터링된 파일을 참조하는 파일 디스크립터가 닫혔습니다. 닫힌 파일 디스크립터에 쓰기 액세스 권한이 있었습니다. 이 노트와 NOTE_CLOSE는 강제로 파일을 닫을 때(unmount(2) 또는 revoke(2)로) 활성화되지 않습니다. 대신, 이러한 이벤트에 대해 NOTE_REVOKE가 전송됩니다.
- NOTE_DELETE: 디스크립터로 참조된 파일에서 unlink() 시스템 호출이 수행되었습니다.
- NOTE_EXTEND: 정규 파일의 경우 디스크립터로 참조된 파일이 확장되었습니다. 디렉터리의 경우 디렉터리 항목이 이름 바꾸기 작업의 결과로 추가 또는 제거되었습니다. 디렉터리 내에서 이름이 변경되면 NOTE_EXTEND 이벤트는 보고되지 않습니다.
- NOTE_LINK: 파일의 링크 수가 변경되었습니다. 특히 디스크립터로 참조된 디렉터리 내에서 하위 디렉터리가 생성되거나 삭제되었을 경우 NOTE_LINK 이벤트가 보고됩니다.
- NOTE_OPEN: 디스크립터로 참조된 파일이 열렸습니다.
- NOTE_READ: 디스크립터로 참조된 파일에서 읽기가 발생했습니다.
- NOTE_RENAME: 디스크립터로 참조된 파일이 이름이 바뀌었습니다.
- NOTE_REVOKE: 파일의 액세스가 revoke(2) 또는 기반이 되는 파일 시스템이 마운트 해제로 인해 취소되었습니다.
- NOTE_WRITE: 디스크립터로 참조된 파일에 쓰기가 발생했습니다

>반환 시 fflags에는 필터를 트리거한 이벤트가 포함됩니다.

- **EVFILT_PROC** : 이 필터는 모니터링할 프로세스 ID를 식별자로 사용하며 fflags에 모니터링할 이벤트를 지정하고 프로세스가 요청한 하나 이상의 이벤트를 수행할 때 반환합니다. 프로세스가 일반적으로 다른 프로세스를 볼 수 있는 경우 해당 프로세스에 이벤트를 연결할 수 있습니다. 모니터링할 수 있는 이벤트는 다음과 같습니다:

- NOTE_EXIT: 프로세스가 종료되었습니다. 종료 상태는 wait(2)로 반환되는 상태와 동일한 형식으로 data에 저장됩니다.
- NOTE_FORK: 프로세스가 fork()를 호출했습니다.
- NOTE_EXEC: 프로세스가 execve(2) 또는 유사한 호출을 통해 새 프로세스를 실행했습니다.
- NOTE_TRACK: fork() 호출을 통해 프로세스를 따라갑니다. 부모 프로세스는 원래 이벤트와 동일한 fflags를 사용하여 자식 프로세스를 모니터링하기 위해 새 kevent를 등록합니다. 자식 프로세스는 fflags에서 NOTE_CHILD가 설정된 이벤트를 신호로 보내고 데이터에 부모 PID를 포함합니다. 부모 프로세스가 새 kevent를 등록하지 못할 경우(일반적으로 리소스 제한으로 인한 경우) NOTE_TRACKERR이 fflags로 설정된 이벤트를 신호로 보내고 자식 프로세스는 NOTE_CHILD 이벤트를 신호로 보내지 않습니다.

>반환 시 fflags에는 필터를 트리거한 이벤트가 포함됩니다.

- **EVFILT_PROCDESC** : 이 필터는 pdfork(2)로 생성된 프로세스 디스크립터를 식별자로 사용하며 fflags에 모니터링할 이벤트를 지정하고 연결된 프로세스가 요청한 하나 이상의 이벤트를 수행할 때 반환합니다. 모니터링할 수 있는 이벤트는 다음과 같습니다:

- NOTE_EXIT: 프로세스가 종료되었습니다. 종료 상태는 data에 저장됩니다.

>반환 시 fflags에는 필터를 트리거한 이벤트가 포함됩니다.

- **EVFILT_SIGNAL** : 이 필터는 모니터링할 시그널 번호를 식별자로 사용하며 주어진 시그널이 프로세스에 전달될 때 반환합니다. 이 필터는 signal() 및 sigaction() 기능과 공존하며 우선순위가 낮습니다. 필터는 시그널을 프로세스에 전달하려는 모든 시도를 기록하며, 시그널이 SIGCHLD인 경우 무시된 경우에도 필터에서 기록되지 않습니다. 이벤트 알림은 일반적인 시그널 전달 처리 이후에 발생합니다. data에는 마지막 kevent() 호출 이후에 시그널이 발생한 횟수가 반환됩니다. 이 필터는 내부적으로 EV_CLEAR 플래그를 자동으로 설정합니다.

- **EVFILT_TIMER** : 이 필터는 식별자로 식별된 임의의 타이머를 설정합니다. 타이머를 추가할 때 data는 타이머가 활성화될 때의 시점을 지정합니다 (NOTE_ABSTIME의 경우) 또는 타임아웃 기간을 지정합니다. EV_ONESHOT 또는 NOTE_ABSTIME이 지정되지 않은 한 타이머는 주기적입니다. 반환 시 data에는 kevent() 호출 이후에 타임아웃이 만료된 횟수가 포함됩니다. 비모노토닉 타이머의 경우 이 필터는 내부적으로 EV_CLEAR 플래그를 자동으로 설정합니다.

>fflags 인수에는 다음과 같은 플래그가 포함됩니다:

- NOTE_SECONDS: data는 초 단위입니다.
- NOTE_MSECONDS: data는 밀리초 단위입니다.
- NOTE_USECONDS: data는 마이크로초 단위입니다.
- NOTE_NSECONDS: data는 나노초 단위입니다.
- NOTE_ABSTIME: 지정된 만료 시간이 절대적임을 나타냅니다.

fflags가 설정되지 않은 경우 기본값은 밀리초입니다. 반환 시 fflags에는 필터를 트리거한 이벤트가 포함됩니다.

주기적 타이머의 경우 타임아웃이 0으로 지정된 경우 요청된 정밀도의 시간 단위 중 1 단위의 시간이 지난 후에 타임아웃으로 조용히 조정됩니다. 지나간 절대 시간이 지정된 경우 현재 시간이 지정된 것처럼 처리되고 이벤트는 가능한 빨리 발생합니다.

기존 타이머를 다시 추가하면 기존 타이머가 효과적으로 취소되고 (이전 타이머 만료에 대한 기록을 삭제) data 및 fflags에 포함된 새 매개변수를 사용하여 다시 시작됩니다.

시스템 전체에서 제한된 수의 타이머가 있으며 이는 kern.kq_calloutmax sysctl에 의해 제어됩니다.

- **EVFILT_USER** : 이 필터는 사용자 수준 코드에서 트리거되는 사용자 이벤트를 설정합니다. ident로 식별되며 커널 메커니즘과 관련이 없지만 사용자 수준 코드에서 트리거됩니다. fflags의 하위 24비트는 사용자 정의 플래그로 사용될 수 있으며 다음과 같이 조작됩니다:

- NOTE_FFNOP: 입력 fflags를 무시합니다.
- NOTE_FFAND: fflags를 비트 AND 연산합니다.
- NOTE_FFOR: fflags를 비트 OR 연산합니다.
- NOTE_FFCOPY: fflags를 복사합니다.
- NOTE_FFCTRLMASK: fflags를 제어하는 마스크입니다.
- NOTE_FFLAGSMASK: fflags의 사용자 정의 플래그 마스크입니다.

사용자 이벤트는 다음과 같이 출력됩니다:

- NOTE_TRIGGER: 이벤트를 트리거하도록합니다.

>반환 시 fflags의 하위 24비트에는 사용자가 정의한 플래그가 포함됩니다.

#### 취소 동작
- 만약 nevents가 0이 아닌 경우, 즉 함수가 잠재적으로 블로킹될 수 있는 경우, 호출은 취소 지점(cancellation point)입니다. 그렇지 않으면, 즉 nevents가 0인 경우 호출은 취소할 수 없습니다. 취소는 kqueue에 대한 변경 사항이 발생하기 전에만 발생하거나 호출이 차단되고 큐에 대한 변경 사항이 요청되지 않은 경우에만 발생할 수 있습니다.

#### 반환 값
- kqueue() 시스템 호출은 새로운 커널 이벤트 큐를 생성하고 파일 디스크립터를 반환합니다. 커널 이벤트 큐를 생성하는 데 오류가 발생한 경우 -1의 값을 반환하고 errno가 설정됩니다.

- kevent() 시스템 호출은 eventlist에 배치된 이벤트의 수를 반환하며, 이 수는 nevents 값으로 주어진 값까지입니다. changelist의 요소를 처리하는 동안 오류가 발생하고 eventlist에 충분한 공간이 있는 경우, 해당 이벤트는 eventlist에 EV_ERROR 플래그와 데이터에 시스템 오류가 설정된 상태로 배치됩니다. 그렇지 않으면 -1이 반환되고 오류 상태를 나타내는 errno가 설정됩니다. 제한 시간이 경과하면 kevent()는 0을 반환합니다.
#### [**EXAMPLES**](https://man.freebsd.org/cgi/man.cgi?kqueue#end)
- 링크 참고
#### [**ERRORS**](https://man.freebsd.org/cgi/man.cgi?kqueue#end)
kqueue() 시스템 호출이 실패하는 경우:
- [ENOMEM]: 커널이 커널 큐에 충분한 메모리를 할당하지 못한 경우.
- [ENOMEM]: 현재 사용자의 RLIMIT_KQUEUES 제한(see getrlimit(2))을 초과하게 되는 경우.
- [EMFILE]: 프로세스 당 디스크립터 테이블이 가득 찬 경우.
- [ENFILE]: 시스템 파일 테이블이 가득 찬 경우.

kevent() 시스템 호출이 실패하는 경우:
- [EACCES]: 프로세스가 필터를 등록할 권한이 없는 경우.
- [EFAULT]: kevent 구조체를 읽거나 쓰는 동안 오류가 발생한 경우.
- [EBADF]: 지정된 디스크립터가 잘못된 경우.
- [EINTR]: 타임아웃이 만료되기 전이나 이벤트가 반환을 위해 kqueue에 배치되기 전에 시그널이 전달된 경우.
- [EINTR]: 취소 요청이 스레드로 전달되었지만 아직 처리되지 않은 경우.
- [EINVAL]: 지정된 시간 제한 또는 필터가 잘못된 경우.
- [EINVAL]: 이벤트 또는 변경 목록의 지정된 길이가 음수인 경우.
- [ENOENT]: 수정 또는 삭제할 이벤트를 찾을 수 없는 경우.
- [ENOMEM]: 이벤트를 등록할 메모리가 없거나 타이머의 경우 최대 타이머 수가 초과된 경우. 이 최대값은 kern.kq_calloutmax sysctl을 통해 구성 가능합니다.
- [ESRCH]: 연결할 프로세스가 존재하지 않는 경우.

>kevent() 호출이 EINTR 오류와 함께 실패한 경우, changelist의 모든 변경 사항이 적용되었습니다.

