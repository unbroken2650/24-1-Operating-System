# Chapter 4

Subject: Thread



# 4.1 Overview

![https://i.imgur.com/Xr8lWHG.png](https://i.imgur.com/Xr8lWHG.png)

Thread :  CPU 활용의 기본 unit

- 구성 - Thread ID, PC, Registers, Stack
- Threads끼리 코드, 데이터와 같은 OS 자원을 공유

### Multiple threads of control

= 한 번에 여러 가지 일을 하는 것

### Benefits👍 of multithreaded programming

- Responsiveness : 사용자의 응답에 대한 즉각적인 응답
- Resource sharing
- Economy : Process를 생성하는 것보다 메모리와 시간이 줄어듬
- Scalability : 여러 코어에서 병렬로 실행 가능

# 4.2 Multicore Programming

Multicore : 하나의 프로세싱 칩에 여러 개의 컴퓨팅 코어

- Concurrency : 실행 시간 중 항상 task가 실행되는 것 = 동시실행
    - Multicore : 여러 개의 Thread가 병렬적으로 실행 (=Parallelism=simultaneous)

### Challenges

- identifying tasks : task를 개별적으로 분리해야 함
- balance : 각 task가 같은 양의 작업을 수행해야 함
- data splitting : 다른 코어에서 Thread가 동작하도록 데이터를 나누어야 함
- data dependency : synchronization이 필요
- testing and debugging : test와 debug 과정이 어려움

### Amdahl’s Law

$$
speedup \leq \frac{1}{S+\frac{1-S}{N}}
$$

### Types of Parallelism

- Data parallelism : 데이터를 같은 양으로 나누어 Thread에 분배
- Task paralleism : 각 Thread가 서로 다른 task를 수행하게 함
- Hybrid : Data / Task parallelism을 동시에 수행할 수 있음

# 4.3 Multithreading Models

### User / Kernel threads

User threads : kernel 상위에서 kernel의 지원 없이 동작하는 Thread

Kernel threads : kernel의 지원을 받으며 OS에 의해 직접적으로 관리되는 Thread

### Many-to-One Model

![https://i.imgur.com/75vyXqT.png](https://i.imgur.com/75vyXqT.png)

N User - 1 Kernel

- user space에서 thread library를 사용하므로 효율적
- block system call을 사용하면 전체 process가 멈춤
- Multicore system에서 parallel running X

### One-to-One Model

![https://i.imgur.com/GMsmPEJ.png](https://i.imgur.com/GMsmPEJ.png)

1 User - 1 Kernel

- block system call을 사용해도 다른 Thread에 영향 X
- Multiprocessor에서 여러 개의 Thread 동작
- User thread가 많아지면 Kernel thread도 많아지므로 성능이 저하
- Linux, Windows

### Many-to-Many Model

![https://i.imgur.com/vkRiYLA.png](https://i.imgur.com/vkRiYLA.png)

N User - N Kernel

- Multiplex
- thread 개수의 제한이 없음
- implementation이 복잡함

### Two-level Model

![https://i.imgur.com/hTy1y7O.png](https://i.imgur.com/hTy1y7O.png)

Many-to-Many와 One-to-One을 결합

# 4.4 Thread Libraries

User-level library - local function call

Kernel-level library - system call

전역변수는 공유

- POSIX Pthreads - Linux, User or Kernel level
- Windows thread - Kernel-level
- Java thread API

### Synchronous/Asynchronous threading

Synchronous : Child가 끝날 때까지 기다렸다가 Parent 실행

Asynchronous : Parent와 Child가 동시에+독립적으로 실행

# 4.5 Implicit Threading

Compiler와 런타임 Library가 Thread를 관리

### Thread pools

Thread를 생성하고 소멸하는 것에 대한 시간이 필요

→ 프로그램을 수행할 때 여러 개의 Thread를 생성하여 Pool에 위치
→ 요청이 들어오면 Thread Pool의 Thread를 활용하여 동작

### Fork Join Model

![https://i.imgur.com/wlb0RUe.png](https://i.imgur.com/wlb0RUe.png)

Thread Pool의 synchronous버전

### OpenMP

Compiler directive와 API의 집합

- 병렬 프로그래밍과 Shared Memory 환경을 지원
- compiler directive를 통해 병렬로 처리할 수 있는 부분을 탐색

### GCD(Grand Central Dispatch)

macOS, iOS에서 사용하는 런타임 라이브러리, API, language extensions의 집합

- dispatch queue, thread pool

### Intel Thread Building Blocks

C++의 병렬처리 라이브러리

- 병렬 Task를 지정하면 schedular가 thread와 연결

# 4.6 Threading Issues

- fork(), exec()
    
    fork()와 exec()를 연속해서 실행하면 calling thread만 복제
    
    fork()를 실행하고 exec()를 실행하지 않는다면 모든 thread 복제
    
- Signal Handling
    - Synchronous signals - illegal memory access, division by 0
    - Asynchronous singals - event external to a running process
    - Singal delivery in multithreaded programs
        - 모든 thread에 전달 / 특정 thread에 전달
- Thread Cancellation
    
    Thread가 끝나기 전 종료
    
    - Asynchronous / Deferred
    
    ex) `Pthread_testcancel()` @ POSIX, C
    
- TLS(Thread-Local Storage)
    
    데이터를 공유하는 대신 복사하여 사용
    
    `static __thread int threadID;`
    
- Scheduler Activations
    - LWP(Lightweight process) : User thread와 Kernel thread 사이의 intermediate data structure
        
        ex) blocking thread → upcall → upcall handler on new LWP
        
        ![https://i.imgur.com/B4NZ0Jl.png](https://i.imgur.com/B4NZ0Jl.png)
        
        - Kernel thread에 존재
        - User thread에게는 가상의 프로세서처럼 동작

# 4.7 OS Examples

### Windows Threads

one-to-one mapping

ETHREAD(executive), KTHREAD(kernel), TEB(thread environment)

![Untitled](Chapter%204%20b5599f1efceb4dc2976190615aa45dec/Untitled.png)

### Linux Threads

fork(), clone()

kernel 자료구조인 `struct`를 사용

다른 자료구조(파일시스템, 메모리 공간, Singal Handler, 파일을 공유할 때)를 가리키는 포인터를 포함함