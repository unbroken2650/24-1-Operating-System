# Chapter 2

Subject: Operating-System Structures


Lecture: 운영체제 (https://www.notion.so/2161ecba840949b59723a257600ae498?pvs=21)


Category: 노트


Progress: 완료


# 2.1 OS Services

![https://i.imgur.com/B5WlvHU.png](https://i.imgur.com/B5WlvHU.png)

# 2.2 User and OS Interface

Command Interpreters / Graphical User Interface / Touch-Screen Interface

Choice of Interface : 개인 취향, 용도에 맞게, Shell Script 이용한 CLI가 GUI보다 빠를 수도

# 2.3 System Calls

= Interface to the services

C나 C++ 또는 어셈블리어로 작성된 함수

ex) input file을 열거나 읽을 때, output file을 생성하거나 작성할 때 (system call과 error handling 필요)

### API

Application Programming Interface

함수와 파라미터, 반환값의 묶음

API의 대표적인 형식: `[return value] [function name]([parameters])`

API → System call 호출

### RTE

API를 이용하는 System call과 관련된 코드들을 모아놓은 것

### 종류

- Process control
    - 프로세스 관련 / 프로세스 속성 관련 / 이벤트 관련 / 메모리 관련
- File management
    - 파일 관련 / 파일 속성 관련
- Device Management
    - 장치 관련  / 장치 속성 관련 / 장치를 논리적으로 attach/detach
    - File management와 유사
- Information maintenance
    - 시간, 날짜, 시스템 데이터 관련
- Communications
    - 통신 연결, 메시지, 상태 정보 전달, 원격 장치
- Protection
    - 파일 허가

# 2.5 Linkers and Loaders

### Compiler

source program → object file (main.o)

object file 

- elf format(UNIX, Linux), complied machine code와 symbol table을 포함
- pe format (Windows), Mach-O format(MacOS)

### Linker

object files → executable file (main)

### Loader

executable file + DLL → program in memory (메모리에 로딩)

DLL(dynamically linked libraries) : 프로그램에 필요한 별도의 함수들

### Relocation

프로그램을 final 주소에 할당

주소와 맞추기 위해 프로그램의 코드와 데이터를 수정

# 2.6 Why Applications are OS Specific

**프로그램들이 OS에 따라 동작하지 않는 이유**

- OS마다 system call이 다름

**다양한 OS에서 동작하는 프로그램들**

- interpreted language ex) Python
- virtual machine이 있는 경우 ex) Java
- standard language나 API

# 2.8 OS Structure

### Monolithic Structure

![https://i.imgur.com/9BSLIyU.png](https://i.imgur.com/9BSLIyU.png)

모든 기능이 Kernel에 포함되어 있음

- 구현이나 확장이 어려움
- 성능이 우수함

ex) original UNIX

### Layered Approach

![https://i.imgur.com/DtKF2ez.png](https://i.imgur.com/DtKF2ez.png)

- 각 레이어들이 분리되어있음

### Microkernels

불필요한 부분 제거 → 작은 kernel과 응용 프로그램으로 구성

![https://i.imgur.com/0AeyCKa.png](https://i.imgur.com/0AeyCKa.png)

### Modules

LKM(loadable kernel modules)을 이용

Kernel에 core components가 있고, 동적으로 module을 로드하여 추가적인 서비스를 수행

↔ layered system은 바로 아래 layer의 모듈만 호출할 수 있음

↔ microkernel approach : message passing을 할 필요 없음

### Hybrid Systems

여러 개 혼합하여 사용

ex) Linux : monolithic + modular

# 2.9 Building and Booting an OS

1. BIOS/UEFI
2. Bootstrap이 kernel을 찾고
3. kernel이 메모리에 로드되고 시작
4. kernel이 하드웨어를 초기화
5. root file system이 마운트됨