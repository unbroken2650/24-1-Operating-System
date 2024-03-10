# Chapter 1

Subject: Introduction
Lecture: 운영체제 (https://www.notion.so/2161ecba840949b59723a257600ae498?pvs=21)
Category: 노트
Progress: 공부중

- Content

**Objectives**

- General organization of a computer system & the role of interrupts
- Components in a modern multiprocessor computer system
- The transition form user mode to kernel mode
- How OSs are used in various computing environments
- Exs of free and open-source OSs

## What Operating Systems Do

Computer System = Hardware + OS + Application programs + User

## Operating System(OS)

컴퓨터의 하드웨어를 제어하고 다양한 사용자와 다양한 App에 맞추어 편성하는 소프트웨어

### User View

![https://i.imgur.com/ZDZJMiR.png](https://i.imgur.com/ZDZJMiR.png)

User’s View는 컴퓨터가 사용되는 인터페이스에 따라 달라진다.

- 일반적인 사용의 경우 사용이 쉽도록
- 임베디드 컴퓨팅의 경우 사용자와의 접촉이 없음

최근 모바일 기기의 사용이 늘면서 터치 스크린이나 음성 인식이 도입됨

### System View

OS는 **자원 할당자** 역할+ 에러를 막고 컴퓨터의 부적절한 사용을 막기 위해 프로그램의 실행을 관리하는 **제어 프로그램**

### Definition

- 컴퓨팅 시스템을 원활히 사용하기 위한 방법
- 컴퓨터에서 항상 돌아가는 **Kernel** & **System Programs**으로 구성됨
- 최근 모바일 장치들은 App 개발자들에게 부가적인 기능을 제공하는 **Middleware**도 포함하고 있다

**배우는 이유**

- OS를 개발하기 위해
- 모든 프로그램이 OS 위에서 실행되기 때문에 -> 효율적인 프로그램 제작

## Computer-System Organization

![https://i.imgur.com/jLREcqE.png](https://i.imgur.com/jLREcqE.png)

현대 컴퓨터 시스템 = 하나 이상의 CPU와 수많은 장치 컨트롤러들이 각 요소와 메모리를 공유할 수 있는 **Bus**로 연결되어 있음
각 **장치 컨트롤러**들은 특정한 장치를 담당

- **장치 드라이버**
    - 장치 컨트롤러를 이해하고 OS가 어떤 장치던 동일한 인터페이스로 장치를 이용하도록 만듬
- local buffer & special-purpose register를 가지고 있음

# Interrupts

### Overview

![https://i.imgur.com/ScN0fXz.png](https://i.imgur.com/ScN0fXz.png)

![https://i.imgur.com/0qz5qK6.png](https://i.imgur.com/0qz5qK6.png)

- CPU가 하드웨어에 신호를 요청하고 결과를 전달받기에는 시간이 소요됨
- 그 시간 동안 CPU는 다른 일을 하고 있다가 결과를 수신하면 돌아옴
- 하드웨어는 CPU에게 신호를 보내기 위해 언제나 Interrupt를 발생 (주로 시스템 버스를 사용)
    - ex) 장치 컨트롤러가 동작을 마쳤을 때 장치 드라이버에게 알려주는 동작

> CPU가 인터럽트를 받으면 하던 동작을 멈추고 인터럽트의 서비스 루틴을 실행한다.
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

interrupts are used throughout modern operating systems to handle asynchronous events (and for other purposes we will discuss throughout the text). Device controllers and hardware faults raise interrupts. To enable the most urgent work to be done first, modern computers use a system of interrupt priorities. Because interrupts are used so heavily for time-sensitive
processing, efficient interrupt handling is required for good system performance.