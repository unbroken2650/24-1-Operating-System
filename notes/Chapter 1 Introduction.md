# Chapter 1

# W1-1

**Objectives**

- General organization of a computer system & the role of interrupts
- Components in a modern multiprocessor computer system
- The transition form user mode to kernel mode
- How OSs are used in various computing environments
- Exs of free and open-source OSs

## What Operating Systems Do

Computer System = Hardware + OS + Application programs + User

### Operating System(OS)

컴퓨터의 하드웨어를 제어하고 다양한 사용자와 다양한 App에 맞추어 편성하는 소프트웨어

### User View

![](https://i.imgur.com/ZDZJMiR.png)

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

### 배우는 이유

- OS를 개발하기 위해
- 모든 프로그램이 OS 위에서 실행되기 때문에 -> 효율적인 프로그램 제작

# W1-2

## Computer-System Organization

![](https://i.imgur.com/jLREcqE.png)

현대 컴퓨터 시스템 = 하나 이상의 CPU와 수많은 Device Controller들이 각 요소와 메모리를 공유할 수 있는 **Bus**로 연결되어 있음
각 Device Controller들은 특정한 장치를 담당

- **Device Driver**가 있음
  - Device Controller를 이해하고 OS가 어떤 장치던 동일한 인터페이스로 장치를 이용하도록 만듬
- local buffer & special-purpose register를 가지고 있음

### Interrupts

![](https://i.imgur.com/ScN0fXz.png)

![](https://i.imgur.com/0qz5qK6.png)

- CPU가 하드웨어에 신호를 요청하고 결과를 전달받기에는 시간이 소요됨
- 그 시간 동안 CPU는 다른 일을 하고 있다가 결과를 수신하면 돌아옴
- 하드웨어는 CPU에게 신호를 보내기 위해 언제나 Interrupt를 발생 (주로 시스템 버스를 사용)
  - ex) Device Controller가 동작을 마쳤을 때 Device Driver에게 알려주는 동작

> CPU가 Interrupt를 받으면 하던 동작을 멈추고 interrupt의 서비스 루틴을 실행한다.

Interrupt가 올바른 서비스 루틴으로 이동하려면
일반 루틴 호출 -> Interrupt 정보 검사 -> Interrupt와 관련이 있는 핸들러 호출

인터럽트 서비스 루틴의 포인터를 담은 테이블은 low memory에 저장되어 있음

인터럽트는 현재 상태를 **저장**해둔 다음에 종료 후 되돌려놓는다.

인터럽트가 실행되기 전 저장된 원래 주소는 PC로 로드된 후 인터럽트 종료 시 실행된다.
