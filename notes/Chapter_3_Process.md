# Chapter 3

Subject: Process



# 3.1 Process Concept

### Process

Process : 디스크에 저장되어 있는 데이터인 파일이 메모리로 로드되어 Active 상태가 된 것

PC(Program Counter)와 프로세서의 레지스터를 이용하여 Process의 현재 상태를 표현

- state : new, ready, running, waiting, terminated

Process의 메모리 구조

- text - 실행가능한 코드
- data - 전역변수
- heap - 런타임 중 동적으로 할당되는 부분
- stack - 임시 파일(function parameter, 반환할 주소, 지역변수) 저장소

### PCB

![https://i.imgur.com/cT48ll5.png](https://i.imgur.com/cT48ll5.png)

Process Control Block

프로세스와 관련된 정보를 포함

process number : pid

# 3.2 Process Scheduling

### Process scheduler

어떤 프로세스를 실행할지 결정하는 역할

- CPU를 최대한으로 사용하기 위해 multiprogramming 수행
    - degree of multiprogramming : 메모리에 로드할 수 있는 프로세스의 개수
- frequent switching을 위해 time sharing 수행

Ready queue와 Wait queue 이용

Process는 사용하는 장치의 종류에 따라 CPU-bound와 I/O bound로 나눌 수 있음

### CPU Scheduling

Ready Queue에서 Process를 선택하여 CPU 코어에 할당

### Context Switch

![https://i.imgur.com/R71FLlT.png](https://i.imgur.com/R71FLlT.png)

Context : CPU 레지스터 값, Process 상태, PCB의 메모리 관리 정보

과정 : Process0의 state 저장 → Process1의 state 로드 → Process1 끝나면 state 저장 → Process0의 state 로드

# 3.3 Operations on Processes

### Process Creation

Process가 만들어지면 pid(Unique process identifier)를 이용하여 Process에 접근

Parent의 PCB를 복사하여 Child를 생성

Parent Process의 실행

- Child Process와 함께 실행
- Child Process가 끝날 때까지 기다렸다가 실행

![https://i.imgur.com/ixE0Ocm.png](https://i.imgur.com/ixE0Ocm.png)

### Process Termination

1. exit() system call을 Child Process에서 실행
    
    → 메모리, 파일, I/O 버퍼를 할당해제
    
    → Parent Process에 상태값을 반환
    
2. 다른 Process에 의해 terminated

# 3.5 IPC in Shared-Memory Systems

IPC : InterProcess Communication

![https://i.imgur.com/qOFqhGZ.png](https://i.imgur.com/qOFqhGZ.png)

### shared memory

여러 프로세스가 memory에 접근할 수 있도록 Protection을 제거

# 3.6 IPC in Message-Passing Systems

### Message passing

주소 영역을 공유하지 않고 데이터를 동기화할 수 있음

`send(message)`와 `receive(message)`를 사용

### Direct

제한적인 모듈성 : `send`, `receive`에 Process가 정해져있기 때문에 수정이 용이하지 않음

- symmetry
    - `send(P, message)` : P에게 `message`를 전송
    - `receive(Q, message)` : Q로부터 `message`를 수신
    - 두 프로세스 사이에 링크 1개만 존재
- asymmetry
    - send는 동일
    - `receive(id, message)` : 수신은 `message`의 `id`만 사용

### Indirect

mailbox : messages를 담고 있는 객체

`send(A, message)` : `mailbox` A에 `message`를 전송

`receive(A, message)` : `mailbox` A에서 `message`를 수신

`mailbox`와 Process 사이의 링크가 개별적으로 존재

### Synchronization

- blocking send : `message`가 모두 수신될 때까지 다른 `message`를 보내지 않음
- nonblocking send : `message`를 전송하고 수신 완료 여부와 관계없이 다른 `message`를 바로 전송
- blocking receive : `message`가 수신될 때까지 다른 `message`를 수신하지 않음
- nonblocking receive : `message`수신 시도를 항상 수행

각 send/receive를 함께 사용 가능

### Buffering

IPC를 위한 Queue implementation

- Zero capacity : waiting `message` 없이 blocking만 수행
- Bounded capacity : 정해진 개수의 `message`만 buffer에서 대기, queue가 가득 차면 송신 불가
- Unbounded capacity : queue에 `message`를 무제한으로 추가 가능

### Example

```c
fd = shm_open(name, O_CREAT | O_RDWR, 0666);
```

- fd : file descriptor
- O_CREAT : name이라는 shared memory가 없다면 생성
- O_RDWR : 읽기/쓰기 가능

```c
ftruncate(fd, 4096); // 메모리의 크기 설정
mmap(); // memory-mapped file 생성
```

# 3.7 Examples of IPC Systems

## Pipe

![https://i.imgur.com/Xh2wgYO.png](https://i.imgur.com/Xh2wgYO.png)

- Read end와 Write end로 구성

종류

- bidirectional / unidirectional : 방향성
- half duplex / full duplex : 동시에 전송 불가능/가능(채널 2개 필요)
- relationship O / X

### Ordinary Pipes

- unidirectional
- 외부에서 접근 불가
- Parent-Child 간의 IPC에서 사용
- fd[0] - read end, f[1] - write end로 구성
- IPC를 수행할 때만 존재

### Named Pipes

- bidirectional(half-duplex)
- 여러 Process가 통신에 사용 가능
- IPC가 끝나도 계속 존재

ex) `mkfifo()`- UNIX/Linux