# https://man.freebsd.org/cgi/man.cgi?kqueue
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
