https://wiki.netbsd.org/tutorials/kqueue_tutorial/

# pair
이벤트는 <ident, filter> 쌍으로 식별된다.
ident 는 대상에 따라 descriptor(file, socket, stream), process ID, signal number 일 수 있다.
filter는 각 이벤트를 처리하는 데 사용되는 커널 필터를 식별한다.

예를 들어,
socket에서 읽을 수 있는 데이터가 있을 때 알림을 받으려면 <sckfd, EVFILT_READ> 형식의 이벤트를 지정해야 한다.
프로세스의 활동을 모니터링하려면 <pid, EVFILT_PROC> 튜플이 필요하다.
kqueue에는 동일한 <ident, filter>를 가진 이벤트는 하나만 있을 수 있다.

# flags
이벤트를 디자인한 후에는 이벤트를 kqueue에 추가할지 여부를 결정해야 한다.
