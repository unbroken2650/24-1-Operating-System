# Chapter 1

Subject: Introduction

Lecture: 운영체제

Category: 노트

Progress: 완료


### **Objectives**

- General organization of a computer system & the role of interrupts
- Components in a modern multiprocessor computer system
- The transition form user mode to kernel mode
- How OSs are used in various computing environments
- Exs of free and open-source OSs

# 1.1 What Operating Systems Do

Computer System = Hardware + OS + Application programs + User

## Operating System(OS)

컴퓨터의 하드웨어를 제어하고 다양한 사용자와 다양한 App에 맞추어 편성하는 소프트웨어

### User View

![https://i.imgur.com/ZDZJMiR.png](https://i.imgur.com/ZDZJMiR.png)

User’s View는 컴퓨터가 사용되는 인터페이스에 따라 달라진다.

- 일반적인 사용의 경우 사용이 쉽도록(사용편의성) / 동적 성능, 보안
- 임베디드 컴퓨팅의 경우 사용자와의 접촉이 없음

최근 모바일 기기의 사용이 늘면서 터치 스크린이나 음성 인식이 도입됨

### System View

OS는 **자원 할당자** 역할+ 에러를 막고 컴퓨터의 부적절한 사용을 막기 위해 프로그램의 실행을 관리하는 **제어 프로그램**

### Definition

- 컴퓨팅 시스템을 원활히 사용하기 위한 방법

### Components

- 컴퓨터에서 항상 돌아가는 **Kernel** & **System Programs**으로 구성됨
    - Kernel - 항상 돌아가는 프로그램
    - System Programs - Kernel이 필요할 때 실행시키는 프로그램
- 최근 모바일 장치들은 App 개발자들에게 부가적인 기능을 제공하는 **Middleware**도 포함하고 있다

**배우는 이유**

- OS를 개발하기 위해
- 모든 프로그램이 OS 위에서 실행되기 때문에 -> 효율적인 프로그램 제작

# 1.2 Computer-System Organization

![https://i.imgur.com/jLREcqE.png](https://i.imgur.com/jLREcqE.png)

현대 컴퓨터 시스템 = 하나 이상의 CPU와 수많은 장치 컨트롤러들이 각 요소와 메모리를 공유할 수 있는 **Bus**로 연결되어 있음
각 **장치 컨트롤러**들은 특정한 장치를 담당

- **Device driver**
    - Device Controller를 이해하고 OS가 어떤 장치던 동일한 인터페이스로 장치를 이용하도록 만듬
- local buffer & special-purpose register를 가지고 있음

## Interrupts

### Overview

![https://i.imgur.com/ScN0fXz.png](https://i.imgur.com/ScN0fXz.png)

![https://i.imgur.com/0qz5qK6.png](https://i.imgur.com/0qz5qK6.png)

- CPU가 하드웨어에 신호를 요청하고 결과를 전달받기에는 시간이 소요됨
- 그 시간 동안 CPU는 다른 일을 하고 있다가 결과를 수신하면 돌아옴
- 하드웨어는 CPU에게 신호를 보내기 위해 언제나 **Interrupt**를 발생 (주로 시스템 버스를 사용)
    - ex) 장치 컨트롤러가 동작을 마쳤을 때 장치 드라이버에게 알려주는 동작

> CPU가 인터럽트를 받으면 하던 동작을 멈추고 인터럽트의 서비스 루틴을 실행
> 

![https://i.imgur.com/9tu8XFi.png](https://i.imgur.com/9tu8XFi.png)

인터럽트가 올바른 서비스 루틴으로 이동하려면
일반 루틴 호출 -> 인터럽트정보 검사 -> 인터럽트와 관련이 있는 핸들러 호출

인터럽트 서비스 루틴의 포인터를 담은 테이블(인터럽트 벡터)은 low memory에 저장되어 있음

인터럽트는 현재 상태를 **저장**해둔 다음에 종료 후 되돌려놓는다.

### Implementation

**인터럽트가 발생하는 과정**

1. CPU는 실행을 종료할 때마다 IRQ(Interrupt-Request line)를 확인
2. IRQ에 신호가 들어오면(raise) CPU가 인터럽트를 인지하고(catch)
인터럽트의 번호를 읽은 뒤(ditpatch) 인터럽트 핸들러 루틴으로 이동
3. 해당 루틴을 실행(clear)
4. 인터럽트 핸들러는 인터럽트로 인해 변하는 state를 모두 저장해두고 인터럽트가 종료되면 CPU가 이전 작업을 할 수 있도록 함
    - 인터럽트가 실행되기 전 저장된 원래 주소(인터럽트 벡터에 저장)는 PC로 로드된 후 인터럽트 종료 시 실행

**Interrupt Handling의 필요성**

- 중요한 작업을 할 때 인터럽트 핸들링을 미뤄야 한다
- 장치마다 적절한 인터럽트 핸들러로 처리해야 한다
- 인터럽트의 우선순위를 지정해서 긴급한 요청부터 처리해야 한다

⇒ CPU와 인터럽트 컨트롤러 하드웨어로 처리

**두 개의 Interrupt-Request Line**

1. 마스킹 불가능(FIQ)
    
    복구 불가능한 메모리 에러를 처리할 때 사용
    
2. 마스킹 가능(IRQ)
    
    장치 컨트롤러가 서비스를 요청할 때 사용
    
    중요한 작업을 할 때 꺼둘 수 있음
    

**Interrupt Chaining**

컴퓨터는 실제로 여러 디바이스의 인터럽트 핸들러를 처리해야 하기 때문에 **인터럽트 체이닝**을 사용

인터럽트가 발생하면 인터럽트를 처리할 수 있는 핸들러를 찾을 때까지 해당되는 목록의 핸들러들이 **하나씩** **호출**

**Interrupt Priority Levels**

CPU가 낮은 우선순위의 인터럽트를 미루고 높은 우선순위의 인터럽트를 먼저 실행

### Summary

인터럽트는 현대 운영 체제에서 비동기 이벤트를 처리하기 위해 광범위하게 사용되며(그 외의 용도는 본문에서 계속 다룰 예정입니다). 장치 컨트롤러와 하드웨어 오류가 인터럽트를 발생시킵니다. 가장 긴급한 작업을 우선적으로 처리할 수 있도록 현대 컴퓨터는 인터럽트 우선순위 시스템을 사용합니다. 인터럽트는 시간에 민감한 처리를 위해 매우 중요하게 사용되기 때문에, 효율적인 인터럽트 처리는 좋은 시스템 성능을 위해 필수적입니다.

## Storage Structure

### **Memory Hierachy**

**Volatile storage**(휘발성 메모리) = Primary storage

registers > cache > main memory

CPU가 직접 접근

**Nonvolatile storage**(비휘발성 메모리)

- Secondary storage : nonvolatile memory(SSD) > hard-disk drives(HDD)

### Von Neumann architecture

폰 노이만 구조

- 프로그램을 저장하고 실행
- CPU↔register↔memory
- 메모리 주소를 byte단위로 할당하여 접근

### Word

32bit CPU → word = 4byte(32bit) / 64bit CPU → word = 8byte(64bit)

## I/O Structure

운영체제에서 신뢰성과 성능을 결정하는 중요한 부분

### DMA(Direct Memory Access)

메모리에서 I/O 장치에 직접 접근 by device controller

CPU의 간섭 없음

1 interrupt per 1 block

스위치를 사용하거나 공유 시스템 버스를 사용

# 1.3 Computer-System Architecture

## Single-Processor Systems

1코어 CPU + 특수 프로세서

## Multiprocessor Systems

Multiple cores(PC) / CPU chips(서버)

시스템 버스, 메모리, 주변장치를 공유

(N개의 코어 throuhput) < (1개의 코어 throughput) * N

### SMP(Symmetric multiprocessing)

![Untitled](Chapter%201%20ab5de36306fb4f2b9ecb9be8f7051d83/Untitled.png)

프로세스를 동시에 실행 

### Multicore Processor

![Untitled](Chapter%201%20ab5de36306fb4f2b9ecb9be8f7051d83/Untitled%201.png)

### Non-uniform memory access(NUMA)

![Untitled](Chapter%201%20ab5de36306fb4f2b9ecb9be8f7051d83/Untitled%202.png)

Blade servers : 여러 개의 보드

Clustered systems(네트워크 연결) : 고장 잘 안 남, [hot-standby(asym) / monitor(sym)], HPC

# 1.4 Operating-System Operations

Bootstrap program → Kernel

**Bootstrap program**

컴퓨터의 전원을 켰을 떄 가장 먼저 켜지는 프로그램

하드웨어를 확인하고 운영체제&kernel 로드

BIOS → UEFI, 펌웨어 형태 in 플래시 메모리

**Kernel**

부팅 시 메모리로 로드됨

system daemon이 실행된다

**Trap(exception)**

SW에 의해 발생하는 인터럽트(그냥 인터럽트는 HW(메모리)에 의해 발생)

ex) error, user program

### Multiprogramming & Multitasking

**Multiprogramming** : 한 번에 여러 프로그램을 실행

→ CPU 사용을 늘림

- 프로세스가 대기 상태(CPU가 Idle 상태)일 때 context switching
= 실행하는 프로세스를 다른 프로세스로 변경

Multitasking : logical extension

- Context switching을 수시로 진행

![Untitled](Chapter%201%20ab5de36306fb4f2b9ecb9be8f7051d83/Untitled%203.png)

### Dual-Mode and Multimode Operation

**Kernel mode** : 잘못된 프로그램으로부터 OS를 보호하기 위한 모드
ex) I/O control

하드웨어는 kernel mode에서 시작함

**User Mode** : 유저 프로그램이 실행될 수 있는 모드

**Multimode** : 여러 개의 모드를 활용

![Untitled](Chapter%201%20ab5de36306fb4f2b9ecb9be8f7051d83/Untitled%204.png)

### Timer

무한 반복문이나 대기에서 빠져나오기 위해 주기적으로 인터럽트를 발생

ex) HZ = # of interrupts per second

# 1.5 Resource Management

- Process Management : 프로세스에 필요한 자원(CPU, 메모리, 파일, 입출력 장치) 관리
- Memory Management : 메모리 공간과 프로세스를 추적하여 할당/제거
- File-System Management : 저장공간의 논리적 view인 File을 관리
- Mass-Storage Management : HDD, SSD 관리
- I/O System Management : 특정 하드웨어를 위한 관리
→ I/O Subsystem : CPU쪽은 동일하고 장치 쪽에서 Device driver
- Cache Management

![https://i.imgur.com/xO8l6he.png](https://i.imgur.com/xO8l6he.png)

# 1.6 Security and Protection

### Protection

프로세스나 사용자가 자원에 접근하는 것을 통제하는 것
허가/무허가 사용을 구분

### Security

내/외부 공격을 막는 것

### Distinguishing users

user identifier(user ID)를 활용하여 사용자를 관리

group으로 묶어서 하기도 함

# 1.7 Virtualization

Virtualization : 한 컴퓨터의 하드웨어를 여러 실행환경으로 추상화하는 것

Emulation : 소프트웨어를 이용하여 하드웨어를 시뮬레이션하는 것
= heavy price & performance degradation

# 1.8 Distributed Systems

여러 컴퓨터 시스템을 모아서 사용하는 것

heterogeneous : 컴퓨터들이 서로 다를 수 있음

NFS(파일이 연결), FTP(컴퓨터가 연결)

# 1.9 Kernel Data Structures

### Lists

데이터가 순차적으로 배열된 것

- singly, doubly, circularly
- 가변적인 크기, 원소 쉽게 삽입/삭제
- 성능상의 단점 : O(n)

### Stacks

Last In, First Out(LIFO) - 후입선출

- push / pop

### Queues

First In, First Out(FIFO) - 선입선출

### Trees

- Hierachy : Parent-Child 관계로 연결된 원소들
- general / binary
- binary search - left child ← parent ← right child
worst case $O(n)$ ⇒ Balanced binary search tree $O(log_2(n))$

### Hash Functions and Maps

Hash Function : 입력값이 함수를 통해 index로 매핑되는 테이블 

Hash collision 발생할 가능성 있음 → Hash map(key-value 관계의 자료 구조)

# 1.10 Computing Environments

**Traditional** : PC, file/print server, laptop → portal to server

- batch → interactive
- time sharing → multiprogramming / multitasking

**Mobile**, **Client-Server**

**Peer-to-Peer**

- centralized service lookup service,  discovery protocol

![https://i.imgur.com/Dzbfu8W.png](https://i.imgur.com/Dzbfu8W.png)

**Cloud**

연산 장치, 저장소, 응용 프로그램을 서비스로 제공

ex) Saas, PaaS, IaaS

![https://i.imgur.com/Y8CJ2Co.png](https://i.imgur.com/Y8CJ2Co.png)

**Real-Time Embedded Systems**

특정한 task를 위한 시스템, 제한된 기능을 제공, UI가 없거나 최소한

- Real-Time OS

# 1.11 Free and Open-Source Operating Systems

### Free software

무료 사용/재배포/수정

### Open-source software

배포나 코드를 공개

### Closed-source

MS Windows

### Hybrid approach

Apple macOS

### GNU/Linux

GNU + Linux, 다양한 배포판

### BSD UNIX

FreeBSD, Solaris

# Summary

![https://i.imgur.com/0cTu7pU.png](https://i.imgur.com/0cTu7pU.png)

![https://i.imgur.com/QrblCEk.png](https://i.imgur.com/QrblCEk.png)
