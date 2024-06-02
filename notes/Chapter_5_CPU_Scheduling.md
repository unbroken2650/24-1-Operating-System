# Chapter 5

Subject: CPU Scheduling



# 5.1. Basic Concepts

Objective: to maximize CPU utilization 

- 대기 상태일 때 프로세스 실행
- ready queue에 넣어뒀다가 실행

### CPU-I/O Burst Cycle

CPU burst → I/O burst 반복

### CPU Scheduler

ready queue(FIFO)에 있는 프로세스를 선택하고 CPU에 할당

CPU Scheduling이 이루어지는 상황

1. running→waiting
2. running → ready ex) interrupt
3. waiting → ready ex) completion of I/O
4. terminate

**Nonpreemptive**: 프로세스가 계속 CPU를 점유 ex) 1&4

**Preemptive**: state 변경 가능 ex) 2&3

- race condition 발생 가능

### Dispatcher

![https://i.imgur.com/H23SQcU.png](https://i.imgur.com/H23SQcU.png)

프로세스가 CPU의 core에 대한 제어할 수 있게

- switching context
- switching to user mode
- PC를 user program으로 이동

# 5.2. Scheduling Criteria

스케쥴링 알고리즘을 선택하는 기준

- CPU 사용: 40-80%
- Throughput: 완료한 프로세스 개수
- Turnaround time: 프로세스 시작부터 완료까지의 시간(wait + execute + I/O)
- **Waiting time**: ready queue에서 대기하는 시간
- Response time: output 출력을 시작한 시간

Optimization: avg measures / 특정 criteria에 초점

# 5.3. Scheduling Algorithms

- 1 CPU Burst per process
- avg waiting time 비교
- 1 core

## FCFS

![https://i.imgur.com/VAuh9Gf.png](https://i.imgur.com/VAuh9Gf.png)

First-Come, First-Served w/ FIFO queue

Nonpreemptive

### Convoy Effect

큰 프로세스가 끝날 때까지 다른 프로세스들이 기다려야 함

## SJF

![https://i.imgur.com/DSZeLLh.png](https://i.imgur.com/DSZeLLh.png)

Shortest-Job-First

avg waiting time이 줄어들도록

Preemptive → SJF, Nonpreemptive → SRTF(Shortest-Remaining-Time-First, 남아 있는 시간이 짧은 걸 먼저 실행)

### Implementation promblem

다음 프로세스의 remaining CPU burst의 길이를 알 수가 없음

→ 이전 프로세스들의 CPU burst의 exp. avg. 를 이용해서 다음 CPU burst을 예측

## Round-Robin Scheduling

![https://i.imgur.com/zyeOHSq.png](https://i.imgur.com/zyeOHSq.png)

Preemptive

Time quantum: 한 번 실행하는 시간

## Priority Scheduling

![https://i.imgur.com/0QzxO67.png](https://i.imgur.com/0QzxO67.png)

우선순위를 지정하여 실행

Preemptive: 들어오는 프로세스가 prioiry가 높다면 대체
Nonpreemptive: 실행하던 프로세스부터

### Starvation

낮은 우선순위의 프로세스가 늦게 실행되는 문제

→ RR과 결합하여 같은 우선순위의 프로세스들을 돌아가면서 실행

## Multilevel Queue Scheduling

![https://i.imgur.com/kCmdRwl.png](https://i.imgur.com/kCmdRwl.png)

임의의 기준에 따라 Queue를 생성하고 Queue 내부에서 따로 스케쥴링을 수행

## Multilevel Feedback Queue Scheduling

![Untitled](Chapter%205%2000bd4bc34ae340ecb4f9012501327e7c/Untitled.png)

feedback에 따라 queue 사이의 프로세스가 이동할 수 있음

params 수가 너무 많이 필요함

# 5.4. Thread Scheduling

Process-contention scope(PCS): thread 라이브러리가 스케쥴링

System-contention scope(SCS): OS가 스케쥴링

# 5.5 Multi-Processor Scheduling

## Multiprocessor

- multicore CPUs / multithreaded cores / NUMA / Heterogeneous multiprocessing

### Approaches

- Asymmetric: Master server가 전담, bottleneck 발생
- Symmetric(SMP): 각 프로세서가 self-scheduling

## Multicore Processors

![https://i.imgur.com/KFXYVkL.png](https://i.imgur.com/KFXYVkL.png)

SMP systems ex) CMT(Chip Multithreading)

memory stall: CPU가 메모리에서 데이터를 읽거나 쓸 때 발생하는 지연 시간으로 인해 프로세서가 대기하는 상태

→ cache miss(much faster than memory)

## Multithreading a processing core

concurrent(not parallel), sharing-aware(동시 실행 필요한 프로세스들은 서로 다른 프로세서에 할당)

![https://i.imgur.com/P1RMOXR.png](https://i.imgur.com/P1RMOXR.png)

1. Course grained
    - long-latency event가 발생할 때까지 실행(주기가 길다)
    - pipeline flushing: thread switching(HW, ↔context switching)에 high cost
2. Fine-grained
    - thread switching @ boundary of instruction cycle (주기가 짧다)
    - no pipeline flushing: small cost

### Load Balancing

모든 프로세서에 workload를 균등하게 배분

1. Push Migration: 주기적으로 load를 체크하여 thread를 이동
2. Pull Migration: Idle 프로세서가 task를 가져와서 실행

### Processor Affinity

같은 프로세서에서 계속해서 thread를 실행 → warm cache

- soft affinity: 반드시 같은 프로세서에서 동작하지는 않음
- hard affinity: 어떤 프로세서에서 실행할지 system call을 활용하여 지정

<aside>
💡 Load Balancing & Processor Affinity 사이의 tradeoff를 찾아야 함

</aside>

### NUMA: non-uniform memory access

![https://i.imgur.com/vnPAcqm.png](https://i.imgur.com/vnPAcqm.png)

### Heterogenous Multiprocessing(HMP)

여러 종류의 프로세서를 함께 사용