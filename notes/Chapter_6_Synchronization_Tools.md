# Chapter 6

Subject: Synchronization Tools



# 6.1. Background

### Race Condition

서로 다른 프로세스들(ex - producer, consumer)이 같은 변수를 사용할 때 **race condition**이 발생

# 6.2. Critical-Section Problem

Critical-Section: shared data를 처리할 수 있는 코드 = race condition이 발생하는 부분

```c
while (true) {
	/* entry section */
		/* critical section */
	/* exit section */
		/* remainder section */
}
```

- **Mutual exclusion**: critical section에는 하나의 프로세스만, 다른 프로세스는 접근 불가
- critical section에 들어가려는 프로세스는 허가 받아야 하며(Progress), 무한 대기는 불가(Bounded waiting)

### 해결법

- Single-core: preemption 없이 순서대로 실행
- OS
    - preemptive: responsive, real-time programming에 적합
    - nonpreemptive:

# 6.3. Peterson’s Solution

software-based solution

```c
// Structure of process P_i
int turn;
boolean flag[2];
while (true) {
	flag[i] = true;
	turn = j;
	while (flag[j] && turn ==j);
		/* critical section */
	flag[i] = false;
		/* remainder section */
```

`$P_i$`가 `flag[i]=true`, `turn=j`로 만들기 전까지 `$P_j$`는 critical section으로 진입할 수 없음

- turn이 0 또는 1이므로 2개 프로세스 중 1개만 실행됨 → Mutual exclusion 해결
- 늦게 도착한 프로세스가 while문에서 대기 → Progress 해결
- 다른 프로세스가 critical section을 탈출하면 무한으로 대기하지 않음 → Bounded waiting 해결

⇒ 하지만 현재는 명령어의 순서를 변경할 수 있음 → critical section에 동시에 두 프로세스가 진입할 수 있음

# 6.4. Hardware Support for Sync

memory model

- strongly ordered: 메모리의 수정이 다른 프로세서에 즉각적으로 보임
- weakly ordered: 메모리의 수정이 다른 프로세서들에 즉시 보이지 않음

### Memory barriers

load와 store가 다른 동작이 이루어지기 전에 끝나도록 보장

### Hardware Instructions

하나의 방해받지않는 유닛으로, word를 테스트, 수정하거나 2개의 word의 내용을 변경

- test_and_set(), compare_and_swap(): 서로 다른 코어에서 동시에 수행 가능

![https://i.imgur.com/Aqf55cb.png](https://i.imgur.com/Aqf55cb.png)

### Atomic Variables

기본 자료형(int, bool)에 대한 atomic operation을 제공

모든 상황에서 race condition을 해결하지 않음 ex) bounded-buffer

# 6.5. Mutex Locks

CS를 보호하고 race condition을 예방

- available(bool 변수)
- `acquire()`: CS에 접근할 수 있는 허가를 받음 / `release()`: CS 다 쓰고 반환
- `acquire()`과 `release()` 모두 atomically 수행
- `acquire()`에서 while문 안에 있을 때 계속 기다려야 함(busy waiting = spinlock)

# 6.6. Semaphores

- Semaphore `S`: 정수 변수
- `wait()`: P, `S≤0`일 때 busy wait → 끝나면 `S--`
- `signal()`: V, `S++`
- 모두 atomically 수행 ex) `wait()`에서 `S≤0`과 `S--` 사이 interruption이 없어야 함

### Counting Semaphore

S: integer value

- `wait()`: count가 0이 될 때까지 감소→리소스를 제한
- `signal()`: count를 증가

### Binary Semaphore

mutex lock과 비슷한 방식

### Implementation

busy waiting을 방지

프로세스가 waiting queue에 들어가서 waiting state가 됨(`S→list`)→다른 프로세스가 `signal()`을 수행하면 waiting에서 ready state로 변경

`S→value`가 음수면 기다리는 프로세스들을 나타낸다

# 6.7. Monitors

프로그래밍을 잘못해서 문제 발생할 수 있음 → race condition & permanent blocking
→ high level 단계의 sync. tool을 사용

monitor: ADT(abstract data type)

- 직접 명령어 추가
- mutual exclusion 보장
- 변수: 인스턴스의 상태 + 함수의 내용 (모니터 내부 지역 변수/함수)
- condition construct를 통해 특정 작업을 수행할 수 있음