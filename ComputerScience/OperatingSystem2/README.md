# **OperatingSystem2** 💿

### 참고자료

- 운영체제와 정보기술의 원리 (반효경 저)
- KOCW 강의 [링크](http://www.kocw.net/home/search/kemView.do?kemId=1046323)

## 목차
>- [운영체제](#운영체제)
>>- [운영체제란](#운영체제란)
>>- [운영체제의 분류](#운영체제의-분류)
>- [시스템 구조 및 프로그램 실행](#시스템-구조-및-프로그램-실행)
>>- [컴퓨터 시스템 구조](#컴퓨터-시스템-구조)
>- [프로세스](#프로세스)
>>- [프로세스의 개념](#프로세스의-개념)
>>- [프로세스의 상태](#프로세스의-상태)
>>- [Process Control Block (PCB)](#process-control-block-pcb)
>>- [문맥 교환 (Context Switch)](#문맥-교환-context-switch)
>>- [프로세스를 스케줄링하기 위한 큐](#프로세스를-스케줄링하기-위한-큐)
>>- [스케줄러 (Scheduler)](#스케줄러-scheduler)
>>- [스레드 (Thread)](#스레드-thread)
>- [프로세스 관리](#프로세스-관리)
>>- [프로세스 생성](#프로세스-생성)
>>- [프로세스 종료](#프로세스-종료)
>>- [프로세스와 관련한 시스템 콜](#프로세스와-관련한-시스템-콜)
>>- [프로세스 간 협력](#프로세스-간-협력)
>- [CPU 스케줄링](#cpu-스케줄링)
>>- [CPU 스케줄링 개요](#cpu-스케줄링-개요)
>>- [CPU 스케줄링 알고리즘 종류](#cpu-스케줄링-알고리즘-종류)
>>- [스케줄링 성능 척도](#스케줄링-성능-척도)
>>- [멀티 프로세서 스케줄링](#멀티-프로세서-스케줄링)
>>- [실시간 스케줄링](#실시간-스케줄링)
>>- [스레드 스케줄링](#스레드-스케줄링)
>>- [알고리즘 평가방법](#알고리즘-평가방법)
>- [프로세스 동기화](#프로세스-동기화)
>>- [프로세스 동기화 개요](#프로세스-동기화-개요)
>>- [race condition](#race-condition)
>>- [임계구역 문제 (The Critical-Section Problem)](#임계구역-문제-the-critical-section-problem)
>- [메모리 관리](#메모리-관리)
>>- [Logical vs Physical Address](#logical-vs-physical-address)
>>- [주소 바인딩 (Address Binding)](#주소-바인딩-address-binding)
>>- [Memory-Management Unit (MMU)](#memory-management-unit-mmu)
>>- [Some Terminologies (일부 용어)](#some-terminologies-일부-용어)
>>- [물리 메모리 관리](#물리-메모리-관리)
>>- [Contiguous Allocation (연속 할당)](#contiguous-allocation-연속-할당)
>>- [Noncontiguous allocation (불연속 할당)](#noncontiguous-allocation-불연속-할당)


<br>

---

## 운영체제

>### 운영체제란
>- 운영체제(Operating System, OS)란
>>- 컴퓨터 하드웨어 바로 위에 설치되어 사용자 및 다른 모든 소프트웨어와 하드웨어를 연결하는 소프트웨어 계층
>>- 한마디로 컴퓨터 시스템의 자원을 효율적으로 관리하는것, 자원 관리자
>- 운영체제의 목표
>>- 컴퓨터 시스템을 편리하게 사용할 수 있는 환경을 제공
>>>- 운영체제는 동시 사용자/프로그램들이 각각 독자적 컴퓨터에서 수행되는것 같은 환경을 제공
>>>- 하드웨어를 직접 다루는 복잡한 부분을 운영체제가 대행
>>- 컴퓨터 시스템의 자원을 효율적으로 관리
>>>- 프로세서, 기억장치, 입출력 장치 등의 효율적 관리
>>>>- 사용자간의 형평성 있는 자원 분배
>>>>- 주어진 자원으로 최대한의 성능을 내도록
>>>- 사용자 및 운영체제 자신의 보호
>>>- 프로세스, 파일, 메시지 등을 관리

<br>

[목차로 이동](#목차)

>### 운영체제의 분류
>- 동시 작업 가능 여부
>>- 단일 작업(single ttasking)
>>>- MS-DOS 프롬프트 상에서는 한 명령의 수행을 끝내기 전에 다른 명령을 수행시킬 수 없음
>>- 다중 작업(multi tasking)
>>>- UNIX, MS Windows 등에서는 한 명령의 수행이 끝나기 전에 다른 명령이나 프로그램을 수행할 수 있음
>- 사용자의 수
>>- 단일 사용자
>>>- MS-DOS, MS Windows
>>- 다중 사용자
>>>- UNIX, NT server
>- 처리 방식
>>- 일괄 처리(batch processing)
>>>- 작업 요청의 일정량을 모아서 한꺼번에 처리
>>>- 작업이 완전 종료될 때까지 기다려야 함
>>- 시분할(time sharing)
>>>- 여러 작업을 수행할 때 컴퓨터 처리 능력을 일정한 시간 단위로 분할하여 사용
>>>- 일괄 처리 시스템에 비해 짧은 응답 시간을 가짐
>>- 실시간(Realtime OS)
>>>- 정해진 시간 안에 어떠한 일이 반드시 종료됨이 보장되어야하는 실시간 시스템을 위한 OS
>>>- 예) 원자로/공장 제어, 미사일 제어, 반도체 장비, 로보트 제어

<br>

[목차로 이동](#목차)

---

## 시스템 구조 및 프로그램 실행

>### 컴퓨터 시스템 구조
>- Mode bit
>>- 사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호 장치
>>- Mode bit를 통해 하드웨어적으로 두 가지의 operation 모드 지원
>>>- 1 사용자 모드 : 사용자 프로그램 수행
>>>- 0 모니터 모드(커널 모드, 시스템 모드) : OS 코드 수행
>>- 보안을 해칠 수 있는 중요한 명령어는 모니터 모드에서만 수행 가능한 특권명령으로 규정
>>- Interrupt나 Exception 발생시 하드웨어가 mode bit를 0으로 바꿈
>>- 사용자 프로그램에게 CPU를 넘기기 전에 mode bit를 1로 세팅
>- 타이머
>>- 동작
>>>- 정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트를 발생시킴
>>>- 타이머는 매 클럭 틱마다 1씩 감소
>>>- 타이머 값이 0이 되면 타이머 인터럽트 발생
>>>- CPU를 특정 프로그램이 독점하는 것으로부터 보호
>>- 타이머는 time sharing을 구현하기 위해 널리 이용됨
>>- 타이머는 현재 시간을 계산하기 위해서도 사용
>- Device Controller
>>- I/O device controller
>>>- 해당 I/O 장치유형을 관리하는 일종의 작은 CPU
>>>- 제어 정보를 위해 control register, status register를 가짐
>>>- local buffer를 가짐 (일종의 data register)
>>- I/O는 실제 device와 local buffer 사이에서 일어남
>>- Device controller는 I/O가 끝났을 경우 interrupt로 CPU에 그 사실을 알림
>>- 알아두기
>>>- devide driver : 장치 구동기
>>>>- OS 코드 중 각 장치별 처리루틴 -> software
>>>- device controller : 장치 제어기
>>>>- 각 장치를 통제하는 일종의 작은 CPU -> hardware
>- 입출력(I/O)의 수행
>>- 모든 입출력 명령은 특권 명령
>>- 사용자 프로그램의 I/O 방법
>>>- 시스템콜(system call)
>>>>- 사용자 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출하는 것
>>>>- 사용자 프로그램은 운영체제에게 I/O 요청
>>>- trap을 사용하여 인터럽트 벡터의 특정 위치로 이동
>>>- 제어권이 인터럽트 벡터가 가리키는 인터럽트 서비스 루틴으로 이동
>>>- 올바은 I/O 요청인지 확인 후 I/O 수행
>>>- I/O 완료 시 제어권을 시스템콜 다음 명려으로 옮김
>- 인터럽트(Interrupt)
>>- 인터럽트
>>>- 인터럽트 당한 시점의 레지스터와 program counter를 save한 후 CPU의 제어를 인터럽트 처리 루틴에 넘김
>>- Interrupt (넓은 의미)
>>>- Interrupt (하드웨어 인터럽트) : 하드웨어가 발생시킨 인터럽트
>>>- Trap (소프트웨어 인터럽트)
>>>>- Exception : 프로그램이 오류를 범한 경우
>>>>- System call : 프로그램이 커널 함수를 호출하는 경우
>>- 인터럽트 관련 용어
>>>- 인터럽트 벡터
>>>>- 해당 인터럽트의 처리 루틴 주소를 가지고 있음
>>>- 인터럽트 처리 루틴 (Interrupt Service Routine, 인터럽트 핸들러)
>>>>- 해당 인터럽트를 처리하는 커널 함수
>- DMA (Direct Memory Access)
>>- 빠른 입출력 장치를 메모리에 가까운 속도로 처리하기 위해 사용
>>- CPU의 중재 없이 device controller가 device의 buffer storage의 내용을 메모리에 block 단위로 직접 전송
>>- 바이트 단위가 아니라 block 단위로 인터럽트를 발생시킴

<br>

[목차로 이동](#목차)

---

## 프로세스

>### 프로세스의 개념
>- 프로세스는 하나의 실행중인 프로그램
>- 프로세스 문맥(context)
>>- CPU 수행 상태를 나타내는 하드웨어 문맥
>>>- Program Counter
>>>- 각종 register
>>- 프로세스의 주소 공간
>>>- code, data, stack
>>- 프로세스 관련 커널 자료 구조
>>>- PCB(Process Control Block)
>>>- Kernel stack

<br>

[목차로 이동](#목차)

>### 프로세스의 상태
>- 프로세스는 상태(state)가 변경되며 수행됨
>>- Running
>>>- CPU를 잡고 instruction을 수행중인 상태
>>- Ready
>>>- CPU를 기다리는 상태(메모리 등 다른 조건을 모두 만족하고)
>>- Blocked (wait, sleep)
>>>- CPU를 주어도 당장 instruction을 수행할 수 없는 상태
>>>- Process 자신이 요청한 event(I/O 등)가 즉시 만족되지 않아 이를 기다리는 상태
>>>- 예) 디스크에서 file을 읽어와야 하는 경우
>>- Suspended (stopped)
>>>- 외부적인 이유로 프로세스의 수행이 정지된 상태
>>>- 프로세스는 통째로 디스크에 swap out 됨
>>>- 예시
>>>>- 사용자가 프로그램을 일시 정지시킨 경우(break key)
>>>>- 시스템이 여러 이유로 프로세스를 잠시 중단시킴(메모리에 너무 많은 프로세스가 올라와 있을 때)
>>- New : 프로세스가 생성중인 상태
>>- Terminated : 수행(execution)이 끝난 상태
>>- Blocked : 자신이 요청한 event가 만족되면 Ready
>>- Suspended : 외부에서 resume해 주어야 Active

<br>

[목차로 이동](#목차)

>### Process Control Block (PCB)
>- PCB
>>- 운영체제가 각 프로세스를 관리하기 위해 프로세스당 유지하는 정보
>>- 구성요소(구조체로 유지)
>>>1. OS가 관리상 사용하는 정보
>>>>- Process state, Process ID
>>>>- scheduling information, priority
>>>2. CPU 수행 관련 하드웨어 값
>>>>- Program counter, registers
>>>3. 메모리 관련
>>>>- Code, data, stack의 위치 정보
>>>4. 파일 관련
>>>>- Open file descriptors

<br>

[목차로 이동](#목차)

>### 문맥 교환 (Context Switch)
>- CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정
>- CPU가 다른 프로세스에게 넘어갈 때 운영체제는 다음을 수행
>>- CPU를 내어주는 프로세스의 상태를 그 프로세스의 PCB에 저장
>>- CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어옴

<br>

[목차로 이동](#목차)

>### 프로세스를 스케줄링하기 위한 큐
>- Job queue
>>- 현재 시스템 내에 있는 모든 프로세스의 집합
>- Ready queue
>>- 현재 메모리 내에 있으면서 CPU를 잡아서 실행되기를 기다리는 프로세스의 집합
>- Device queue
>>- I/O device의 처리를 기다리는 프로세스의 집합
>- 프로세스들은 각 큐들을 오가며 수행됨

<br>

[목차로 이동](#목차)

>### 스케줄러 (Scheduler)
>- Long-term scheduler (장기 스케줄러 or job scheduler)
>>- 시작 프로세스 중 어떤 것들을 ready queue로 보낼지 결정
>>- 프로세스에 memory(및 각종 자원)을 주는 문제
>>- time sharing system에는 보통 장기 스케줄러가 없음(무조건 ready)
>- Short-term scheduler (단기 스케줄러 or CPU scheduler)
>>- 어떤 프로세스를 다음 번에 running 시킬지 결정
>>- 프로세스에 CPU를 주는 문제
>>- 충분히 빨라야 함(millisecond 단위)
>- Medium-term scheduler (중기 스케줄러 or Swapper)
>>- 여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄
>>- 프로세스에게서 memory를 뺏는 문제

<br>

[목차로 이동](#목차)

>### 스레드 (Thread)
>- 개요
>>- 스레드 (또는 경량 프로세스)는 CPU 사용의 기본 단위임
>>- 스레드의 구성
>>>- program counter
>>>- register set
>>>- stack space
>>- 스레드가 동료 스레드와 공유하는 부분(=task)
>>>- code section
>>>- data section
>>>- OS resources
>>- 전통적인 개념의 heavyweight process는 하나의 thread를 가지고 있는 task로 볼 수 있음
>>- 다중 스레드로 구성된 태스크 구조에서는 하나의 서버 스레드가 blocked(waiting) 상태인 동안에도 동일한 태스크 내의 다른 스레드가 실행(running)되어 빠른 처리를 할 수 있음
>>- 동일한 일을 수행하는 다중 스레드가 협력하여 높은 처리율(throughtput)과 성능 향상을 얻을 수 있음
>>- 스레드를 사용하면 병렬성을 높일 수 있음
>- 장점
>>- 사용자 입장에서 응답이 빠름
>>- 자원을 공유하여 효율적임
>>- 프로세스를 추가로 만드는 것보다 스레드를 하나 추가하는 것이 경제적임
>>- 멀티프로세서 환경에서 효율적인 실행이 가능함
>- 스레드 구현법
>>- 커널에 의해 지원되는 방식은 커널 스레드
>>- 라이브러리에 의해 지원되는 방식은 유저 스레드

<br>

[목차로 이동](#목차)

---

## 프로세스 관리

>### 프로세스 생성
>- 부모 프로세스가 자식 프로세스를 생성
>- 프로세스의 트리(계층 구조)형성
>- 프로세스는 자원을 필요로 함
>>- 운영체제로부터 받음
>>- 부모와 공유함
>- 자원의 공유
>>- 부모의 자식이 모든 자원을 공유하는 모델
>>- 일부를 공유하는 모델
>>- 전혀 공유하지 않는 모델
>- 수행(Execution)
>>- 부모와 자식은 공존하며 수행되는 모델
>>- 자식이 종료(terminate)될 때까지 부모가 기다리는(wait) 모델
>- 주소 공간(Address space)
>>- 자식은 부모의 공간을 복사함
>>- 자식은 그 공간에 새로운 프로그램을 올림
>- 유닉스의 예
>>- fork() 시스템 콜이 새로운 프로세스를 생성
>>>- 부모를 그대로 복사 (OS data except PID + binary)
>>>- 주소 공간 할당
>>- fork 다음에 이어지는 exec() 시스템 콜을 통해 새로운 프로그램을 메모리에 올림

<br>

[목차로 이동](#목차)

>### 프로세스 종료
>- 프로세스가 마지막 명령을 수행한 후 운영체제에게 이를 알림(exit)
>>- 자식이 부모에게 output data를 보냄 (via wait)
>>- 프로세스의 각 종 자원들이 운영체제에게 반납됨
>- 부모 프로세스가 자식의 수행을 종료시킴(abort)
>>- 자식이 할당 자원의 한계치를 넘어섬
>>- 자식에게 할당된 태스크가 더 이상 필요하지 않음
>>- 부모가 종료(exit)하는 경우
>>>- 운영체제는 부모 프로세스가 종료하는 경우 자식이 더 이상 수행되도록 두지 않음
>>>- 단계적인 종료

<br>

[목차로 이동](#목차)

>### 프로세스와 관련한 시스템 콜
>- fork()
>>- create a child (copy)
>- exec()
>>- overlay new image
>- wait()
>>- sleep until child is done
>- exit()
>>- frees all the resources, notify parent

<br>

[목차로 이동](#목차)

>### 프로세스 간 협력
>- 개요
>>- 독립적 프로세스(Independent process)
>>>- 프로세스는 각자의 주소 공간을 가지고 수행되므로 원칙적으로 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못함
>>- 협력 프로세스(Cooperating process)
>>>- 프로세스 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있음
>>- 프로세스 간 협력 매커니즘 (IPC: Interprocess Communication)
>>>- 메시지를 전달하는 방법
>>>>- message passing : 커널을 통해 메시지 전달
>>>- 주소 공간을 공유하는 방법
>>>>- shared memory : 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 하는 메커니즘
>>>>- thread : thread는 사실상 하나의 프로세스이므로 프로세스 간 협력으로 보기는 어렵지만 동일한 process를 구성하는 thread 간에는 주소 공간을 공유하므로 협력이 가능

<br>

[목차로 이동](#목차)

---

## CPU 스케줄링

>### CPU 스케줄링 개요
>- CPU 버스트, I/O 버스트
>>- 프로그램이 실행될때는 CPU 인스트럭션을 실행하는 단계, IO 작업을 하는 단계를 번갈아가면서 진행됨
>>- CPU 버스트가 짧은 프로그램은 사람과 상호작용이 잦은 프로그램임
>>- CPU 스케줄링이란 레디큐의 프로세스중에서 어떤 프로세스에 CPU를 줄것인지 결정하는것
>- CPU 스케줄링 결정요소
>>- 비선점 - 한번 줬으면 계속 쓰게할것인가
>>- 선점 - 한번 주고 안쓸때는 다른 CPU에 넘길것이냐, 현대 운영체제에서는 대부분 선점식

<br>

[목차로 이동](#목차)

>### CPU 스케줄링 알고리즘 종류
>- FCFS(First come First Served) - 비선점형
>>- 먼저 요청한 프로세스가 먼저 CPU 버스트를 가짐
>>- FIFO 큐를 이용하여 쉽게 관리됨, 평균 대기시간이 대단히 길어질 수 있으므로 효율적이진 않음
>- SJF(Shortest Job First) - 비선점형
>>- 가장 작은 CPU 버스트를 가진 프로세스가 먼저 CPU 버스트를 가짐
>>- 최소 평균 대기 시간이 보장됨
>- SRTF(Shortest-Remaining Time First) - 선점형
>>- 남은 버스트 time보다 더 짧은 CPU 버스트 time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗김
>- SJF와 SRTF의 문제는 CPU 버스트의 길이를 알 수 없기 때문에 CPU 스케줄링 수준에서는 구현할 수 없고, 과거의 CPU 버스트의 소요시간을 통한 예측 근삿값을 통해 구현됨
>- Priority Scheduling - 선점형 또는 비선점형
>>- 우선순위가 가장 높은 프로세스에게 CPU 버스트를 넘긴다
>>- 낮은 우선순위의 프로세스들이 무한히 대기하는 무한 봉쇄 또는 기아 상태가 발생할 수 있는 문제가 있음
>>- 오랫동안 시스템에서 대기하는 프로세스들의 우선순위를 점진적으로 증가시키는 노화를 통해 해결할 수 있음
>- RR(Round Robin) - 선점형
>>- 모든 프로세스에 동일한 시간을 할당
>>- 할당 시간이 끝나면 프로세스는 레디큐의 제일 뒤로 밀려남
>>- 응답시간은 빠르지만 프로세스의 크기에 따라 완료시간이 느려질 수 있음
>>- 프로세스가 전환될 때 컨텍스트 스위칭이 이루어지므로 오버헤드가 커지는 문제가 있음
>- Multilevel Queue
>>- 레디큐를 여러개로 분할
>>>- foreground (interactive) - RR
>>>>- 사람과 상호작용이기때문에 응답시간이 중요하므로 RR
>>>- background (batch - no human interaction) - FCFS
>>>>- CPU만 오랫동안 사용하는 프로세스들이고 응답시간이 중요하지 않으므로 컨텍스트 스위칭 오버헤드를 줄이기위해 FCFS
>>- 큐에대한 스케줄링이 필요
>>>- 고정된 우선순위이므로 적절하게 시간배분을 하지않으면 기아상태 발생가능성이 있음
>>>- 일반적으로 foreground에 80%, background에 20% 할당
>- Multilevel Feedback Queue
>>- Multilevel Queue에서는 프로세스들이 영구적으로 하나의 큐에 할당되므로 융통성이 적음
>>- 프로세스가 다른 큐로 이동 가능, 노화를 이와 같은 방식으로 구현할 수 있음
>>- Multilevel Feedback Queue를 정의하는 매개변수
>>>- 큐의 개수
>>>- 각 큐의 스케줄링 알고리즘
>>>- 프로세스를 상위 큐로 보내는 기준
>>>- 프로세스를 하위 큐로 내쫓는 기준
>>>- 프로세스가 CPU 서비스를 받으려할 때 들어갈 큐를 결정하는 기준

<br>

[목차로 이동](#목차)

>### 스케줄링 성능 척도
>- CPU utilization (이용률) : CPU를 최대한 굴려야한다
>- Throughput (처리량) : 시간당 완료된 프로세스의 개수
>- Turnaround time (소요시간, 변경시간) : 프로세스의 제출시간과 완료 시간의 간격
>- Wating time (대기 시간) : 준비큐에서 얼마나 기다렸느냐
>- Response time (응답 시간) : 하나의 요구를 제출한 후 첫 번째 응답이 나올 때까지의 시간
>>- 응답 시간은 응답을 출력하는 데 걸리는 시간이 아닌 응답이 시작되는 데까지 걸리는 시간임

<br>

[목차로 이동](#목차)

>### 멀티 프로세서 스케줄링
>- 개요
>>- CPU가 여러 개인 경우 스케줄링은 더욱 복잡해짐
>>- Homogeneous processor (대칭 코어 프로세서)
>>>- Queue에 한 줄로 세워서 각 프로세서에가 알아서 꺼내가게 할 수 있음
>>>- 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우에 문제가 복잡해짐
>>- Load sharing (부하 균등화)
>>>- 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유하는 메커니즘 필요
>>>- 별개의 큐를 두는 방법 vs 공동 큐를 사용하는 방법
>>- Symmetric Multiprocessing (SMP)
>>>- 각 프로세서가 각자 알아서 스케줄링 결정
>>- Asymmetric multiprocessing
>>>- 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따름

<br>

[목차로 이동](#목차)

>### 실시간 스케줄링
>- Hard real-time systems (경성 실시간 시스템)
>>- 정해진 시간 안에 반드시 끝내도록 스케줄링해야 함
>- Soft real-time computing (연성 실시간 시스템)
>>- 일반 프로세스에 비해 높은 priority를 갖도록 해야함

<br>

[목차로 이동](#목차)

>### 스레드 스케줄링
>- Local Scheduling
>>- User level thread 의 경우 사용자 수준의 thread library에 의해 어떤 thread를 스케줄할지 결정
>>- 유저 프로세스가 스케줄링의 주체
>- Global Scheduling
>>- Kernel level thread의 경우 일반 프로세스와 마찬가지로 커널의 단기 스케줄러가 어떤 thread를 스케줄할지 결정
>>- 운영체제가 스레드의 존재를 알고 있기 때문에 프로세스 스케줄링 하듯이 운영체제가 알고리즘에 근거하여 결정

<br>

[목차로 이동](#목차)

>### 알고리즘 평가방법
>- Queueing models
>>- 굉장히 이론적인 방법
>>- 확률 분포로 주어지는 arrival rate와 service rate 등을 통해 각종 performance index 값을 계산
>- Implementation (구현) & Measurement (성능 측정)
>>- 실제 시스템에 알고리즘을 구현하여 실제 작업 (workload) 에 대해서 성능을 측정 비교
>- Simulation (모의 실험)
>>- 구현 및 성능 측정 방법보다 좀 더 쉬운 방법
>>- 알고리즘을 모의 프로그램으로 작성후 trace를 입력으로 하여 결과 비교

<br>

[목차로 이동](#목차)

---

## 프로세스 동기화

>### 프로세스 동기화 개요
>- 데이터의 접근
>>1. 저장장소에 데이터
>>2. 저장장소의 데이터를 읽어옴
>>3. 읽어온 데이터를 연산함
>>4. 연산 결과를 저장장소의 데이터로 저장함
>>- 혼자 사용할 때는 문제가 없으나, 여러 프로세스들이 공유 메모리를 사용하거나, 커널 내부 데이터를 사용할 경우 아무런 제약이 없다면 읽어온 시점과 저장한 시점에 따라 결과가 달라지므로 동기화 문제 (race condition, 경쟁상황) 가 발생할 가능성이 있음

<br>

[목차로 이동](#목차)

>### race condition
>- 발생 시점
>>1. kernel 수행 중 인터럽트 발생 시
>>>- 커널코드 running 중 인터럽트가 발생하면 인터럽트 처리루틴이 수행되는데 양쪽 다 커널 코드라 커널 주소 공간을 공유하기 때문에 인터럽트 핸들러의 count 값에 문제가 생김
>>>- 중요한 값을 사용하는 동안에는 인터럽트가 발생해도 인터럽트 처리루틴을 수행하는 것이아니라 disable 시켰다가 작업이 종료된 후 인터럽트 처리루틴을 실행시키는 방법으로 해결함
>>>- 결국은 순서를 정해주면 문제가 해결됨
>>2. 프로세스가 시스템 콜을 하여 커널 모드로 수행중인데 컨텍스트 스위칭이 일어나는 경우
>>>- 두 프로세스의 주소 공간끼리는 데이터 공유가 없으나, 시스템 콜을 하는 동안 커널 주소 공간속 데이터에 접근하게 되어 공유하게 되는데, 작업 중간에 CPU를 선점하면 race condition이 발생함
>>>- 커널 모드에서 수행 중일때는 CPU를 선점하지 않고, 커널 모드에서 사용자 모드로 돌아갈 때 선점하도록 하여 문제를 해결함
>>3. 멀티프로세서에서 공유 메모리 내의 커널 데이터
>>>- 어떤 CPU가 마지막으로 count를 store 했는가에 따라 race condition이 발생함
>>>- 해결방법 1 : 한번에 하나의 CPU만이 커널에 들어갈 수 있게 하는 방법
>>>>- 병목현상이 매우 심함
>>>- 해결방법 2 : 커널 내부에 있는 각 공유데이터 접근할 때마다 그 데이터에 대한 lock / unlock을 하는 방법
>- 문제 정리
>>- 공유 데이터(shared data)의 동시 접근(concurrent access)은 데이터의 불일치 문제(inconsistency)를 발생시킬 수 있음
>>- 일관성(consistency) 유지를 위해서는 협력 프로세스(cooperating process) 간의 실행 순서(orderly execution)를 정해주는 메커니즘이 필요함
>>- Race condition
>>>- 여러 프로세스들이 동시에 공유 데이터를 접근하는 상황
>>>- 데이터의 최종 연산 결과는 마지막에 그 데이터를 다룬 프로세스에 따라 달라짐
>>- race condition을 막기 위해서 concurrent process는 동기화(synchronize) 되어야 함

<br>

[목차로 이동](#목차)

>### 임계구역 문제 (The Critical-Section Problem)
>- 개요
>>- n개의 프로세스가 공유 데이터를 동시에 사용하기를 원하는 경우
>>- 각 프로세스의 code segment에는 공유 데이터를 접근하는 코드인 critical section이 존재
>>- 문제 : 하나의 프로세스가 critical section에 있을 때 다른 모든 프로세스는 critical section에 들어갈 수 없어야 함
>- 프로그램적 해결법의 충족 조건
>>- Mutual Exclusion (상호 배제)
>>>- 프로세스 P가 임계구역 부분을 수행 중이면 다른 모든 프로세스들은 그들의 임계 구역에 들어가면 안됨
>>- Progress (진행)
>>>- 아무도 임계구역에 있지 않은 상태에서 임계구역에 들어가고자 하는 프로세스가 있으면 임계구역에 들어가게 해주어야 함
>>- Bounded Waiting (한정된 대기)
>>>- 프로세스가 임계구역에 들어가려고 요청한 후부터 그 요청이 허용될 때까지 다른 프로세스들이 임계구역에 들어가는 횟수에 한계가 있어야 함
>>- 가정
>>>- 모든 프로세스의 수행 속도는 0보다 크다
>>>- 프로세스들 간의 상대적인 수행 속도는 가정하지 않는다
>- Inital Attempts to Solve Problem
>>- 프로세스들의 일반적인 구조
>>>```c
>>>do {
>>>  entry section
>>>  critical section
>>>  exit section
>>>  remainder section
>>>} while(1);
>>>```

<br>

[목차로 이동](#목차)

---

## 메모리 관리

>### Logical vs Physical Address
>- 메모리라는 것은 주소를 통해 접근하는 매체
>- 메모리에는 주소가 매겨지는데 주소를 크게 두가지로 나누어 볼 수 있음
>>- 논리적인 주소
>>- 물리적인 주소
>- Logical address (= virtual address)
>>- 프로세스마다 독립적으로 가지는 주소 공간
>>- 각 프로세스마다 0번지부터 시작
>>- CPU 가 보는 주소는 logical address 임
>>>- 컴파일된 코드상의 주소는 유지되기 때문에 CPU가 주소를 요청하면 주소변환을 해서 물리 메모리 위치를 찾아서 CPU에 전달하는 방식으로 동작함
>>- 실제 물리 메모리 주소를 추상화한 것
>- Physical address
>>- 메모리에 실제 올라가는 위치
>- 주소바인딩 : 주소를 결정하는 것
>>- Symbolic Address(논리 주소를 변수같은 이름으로 추상화한 것) -> Logical Address(숫자 주소) ->(이 시점이 언제인가? next page) Physical address(실제 메모리의 주소)

<br>

[목차로 이동](#목차)

>### 주소 바인딩 (Address Binding)
>- 논리주소가 물리주소로 변환이 이루어지는 시점은 3가지로 나뉨
>- Compile time binding
>>- 컴파일 시 주소 변환이 이루어지는 것
>>>- 물리적 메모리 주소가 컴파일 시 알려짐
>>- 시작 위치 변경시 재컴파일
>>- 컴파일러는 절대 코드 (absolute code) 생성
>- Load time binding
>>- 실행이 시작될 때 주소 변환이 이루어지는 것
>>>- Loader 의 책임하에 물리적 메모리 주소 부여
>>- 컴파일러가 재배치가능코드 (relocatable code) 를 생성한 경우 가능
>- Execution time binding (=Run time binding)
>>- 수행이 시작된 이후에도 프로세스의 메모리 상 위치를 옮길 수 있음
>>- CPU가 주소를 참조할 때마다 binding 을 점검 (address mapping table)
>>- 하드웨어적인 지원이 필요 (base and limit registers, MMU 라는 하드웨어가 주소변환을 해줘야 함)
>- 예시
>```
># 소스코드 (Symbolic address)
>Add A, B
>Jump C
>A: 100
>B: 330
>C: exit
>
># 소스코드가 컴파일된 실행파일 (Logical address 가 결정됨)
>0 : Add 20, 30
>10: Jump 40
>20: 100
>30: 330
>40: exit
>
># 실행되어 프로세스 메모리들이 물리적 메모리에 매핑
># 변환되는 3가지 시점에 따라 달라지는 소스코드
>
># Compile time binding
>0: Add 20, 30
>10:Jump 40
>20:100
>30:330
># 컴파일 타임에 결정된 항상 같은 논리 주소 위치에 매핑되므로 매우 비효율적임
># 과거 컴퓨터에서 프로그램이 하나만 실행되는 환경에서는 어차피 다른 프로그램이 올라갈일이 없어 사용되었으나 현재는 사용하지않음
>
># Load time binding
>500:Add 20, 30
>510:Jump 40
>520:100
>530:330
># 메모리에 올라갈때 물리 메모리 주소가 결정됨
>
># Run time binding
>300:Add 20, 30
>310:Jump 40
>320:100
>330:330
># 실행 중 변경 가능
>700:Add 20, 30
>710:Jump 40
>720:100
>730:330
>```

<br>

[목차로 이동](#목차)

>### Memory-Management Unit (MMU)
>- MMU
>>- logical address 를 physical address 로 매핑해주는 Hardware device
>- MMU scheme
>>- 사용자 프로세스가 CPU 에서 수행되며 생성해내는 모든 주소값에 대해 base register(=relocation register) 의 값을 더함
>- user program
>>- logical address 만을 다룸
>>- 실제 physical address 를 볼 수 없으며 알 필요가 없음
>- Dynamic Relocation
>>1. CPU 가 logical address 346을 요청
>>2. MMU 의 레지스터 값 relocation register 14000, limit register 3000 값을 이용하여 physical address 로 주소 변환하여 14346 으로 접근
>>>- limit register 는 악의적인 접근을 막기위한 레지스터
>- Hardware Support for Address Translation
>>- 운영체제 및 사용자 프로세스 간의 메모리 보호를 위해 사용하는 레지스터
>>>- Relocation register (=base register) : 접근할 수 있는 물리적 메모리 주소의 최소값
>>>- Limit register : 논리적 주소의 범위
>>- CPU가 메모리까지 접근하는 순서
>>>1. CPU가 logical address 를 MMU로 보냄
>>>2. MMU가 limit register 보다 작다면 relocation register를 더해서 물리주소를 알려줌
>>>>- limit register 보다 크다면 software interrupt 의 일종인 trap: addressing error 발생
>>>3. CPU는 MMU 로부터 logical address + relocation register 인 물리주소를 전달받아서 접근함

<br>

[목차로 이동](#목차)

>### Some Terminologies (일부 용어)
>- Dynamic Loading
>>- 프로세스 전체를 메모리에 미리 다 올리는 것이 아니라 해당 루틴이 불려질 때 메모리에 load 하는 것
>>- memory utilization 의 향상
>>- 가끔씩 사용되는 많은 양의 코드의 경우 유용함
>>>- 예) 오류 처리 루틴
>>- 운영체제의 특별한 지원 없이 프로그램 자체에서 구현 가능 (OS는 라이브러리를 통해 지원 가능)
>>>- 프로그래머가 명시하지않아도 운영체제가 알아서 올리고 내리고 하는 페이징기법과 원래는 다른것이지만 섞어서 쓰기도 함
>>- Loading : 메모리로 올리는 것
>- Overlays
>>- 메모리에 프로세스의 부분 중 실제 필요한 정보만을 올림
>>- 프로세스의 크기가 메모리보다 클 때 유용
>>- 운영체제의 지원없이 사용자에 의해 구현
>>- 작은 공간의 메모리를 사용하던 초창기 시스템에서 수작업으로 프로그래머가 구현
>>>- Menual Overlay
>>>- 프로그래밍이 매우 복잡
>- Swapping
>>- Swapping
>>>- 하나의 프로세스를 일시적으로 메모리에서 backing store 로 쫓아내는 것
>>- Backing store (=swap area)
>>>- 디스크
>>>>- 많은 사용자의 프로세스 이미지를 담을 만큼 충분히 빠르고 큰 저장 공간
>>- Swap in / Swap out
>>>- 일반적으로 중기 스케줄러 (swapper) 에 의해 swap out 시킬 프로세스 선정
>>>- priority-based CPU scheduling algorithm
>>>>- priority 가 낮은 프로세스를 swapped out 시킴
>>>>- priority 가 높은 프로세스를 메모리에 올려 놓음
>>>- Compile time 혹은 load time binding 에서는 원래 메모리 위치로 swap in 해야 함
>>>- Execution time binding 에서는 추후 빈 메모리 영역 아무 곳에나 올릴 수 있음
>>>- swap time 은 대부분 transfer time (swap 되는 양에 비례하는 시간) 임
>- Dynamic Linking
>>- Linking 을 실행 시간 (execution time) 까지 미루는 기법
>>- static linking
>>>- 라이브러리가 프로그램의 실행 파일 코드에 포함됨
>>>- 실행 파일의 크기가 커짐
>>>- 동일한 라이브러리를 각각의 프로세스가 메모리에 올리므로 메모리 낭비 (printf 함수의 라이브러리 코드)
>>- Dynamic linking
>>>- 라이브러리가 실행시 연결(link)됨
>>>- 라이브러리 호출 부분에 라이브러리 루틴의 위치를 찾기 위한 stub 이라는 작은 코드를 둠
>>>- 라이브러리가 이미 메모리에 있으면 그 루틴의 주소로 가고 없으면 디스크에서 읽어옴
>>>- 운영체제의 도움이 필요함
>>>- 일반적으로 shared library 라 부름
>>>>- linux 에선 shared object
>>>>- windows 에선 dll (dynamin linking library)

<br>

[목차로 이동](#목차)

>### 물리 메모리 관리
>- Allocation of Physical Memory
>>- 메모리는 일반적으로 두 영역으로 나뉘어 사용
>>>- OS 상주 영역
>>>>- interrupt vector 와 함께 낮은 주소 영역 사용
>>>- 사용자 프로세스 영역 
>>>>- 높은 주소 영역
>>- 사용자 프로세스 영역의 할당 방법
>>>- Continuous allocation (연속 할당)
>>>>- 각각의 프로세스가 메모리의 연속적인 공간에 적재되도록 하는 것
>>>>>- Fixed partition allocation (고정분할 방식)
>>>>>- Variable partition allocation (가변분할 방식)
>>>- Noncontiguous allocation (불연속 할당)
>>>>- 하나의 프로세스가 메모리의 여러 영역에 분산되어 올라갈 수 있음
>>>>>- Paging
>>>>>- Segmentation
>>>>>- Paged Segmentation

<br>

[목차로 이동](#목차)

>### Contiguous Allocation (연속 할당)
>- Fixed partition allocation (고정분할 방식)
>>- 물리적 메모리를 몇 개의 영구적 분할(partition)로 나눔
>>- 분할의 크기가 모두 동일한 방식과 서로 다른 방식이 존재함
>>- 분할당 하나의 프로그램 적재
>>>- 프로그램의 크기가 분할 크기를 초과하면 외부조각이라는 낭비공간이 발생
>>>- 프로그램의 크기가 분할 크기보다 작으면 내부조각이라는 낭비공간이 발생
>>- 융통성이 없음
>>>- 동시에 메모리에 load 되는 프로그램의 수가 고정됨
>>>- 최대 수행 가능 프로그램 크기 제한
>>- Internal fragmentation(내부 단편화) 발생 (external fragmentation(외부 단편화) 도 발생)
>- Variable partition allocation (가변분할 방식)
>>- 프로그램의 크기를 고려해서 할당
>>- 분할의 크기, 개수가 동적으로 변함
>>- 기술적 관리 기법 필요
>>- External fragmentation 발생
>- Hole
>>- 가용 메모리 공간
>>- 다양한 크기의 hole들이 메모리 여러 곳에 흩어져 있음
>>- 프로세스가 도착하면 수용가능한 hole을 할당
>>- 운영체제는 다음의 정보를 유지
>>>- 할당 공간
>>>- 가용 공간 (hole)
>- Dynamic Storage-Allocation Problem
>>- 가변 분할 방식에서 size n인 요청을 만족하는 가장 적절한 hole을 찾는 문제
>>- First fit
>>>- Size 가 n 이상인 것 중 최초로 찾아지는 hole 에 할당
>>- Best fit
>>>- Size 가 n 이상인 가장 작은 hole 을 찾아서 할당
>>>- Hole 들의 리스트가 크기순으로 정렬되지 않은 경우 모든 hole 의 리스트를 탐색해야함
>>>- 많은 수의 아주 작은 hole 들이 생성됨
>>- Worst fit
>>>- 가장 큰 hole 에 할당
>>>- 모든 리스트를 탐색해야함
>>>- 상대적으로 아주 큰 hole 들이 생성됨
>>- First fit 과 Best fit 이 Worst fit 보다 속도와 공간 이용률 측면에서 효과적인 것으로 알려짐 (실험적 결과)
>- compaction
>>- external fragmentation 문제를 해결하는 한 가지 방법
>>- 사용 중인 메모리 영역을 한군데로 몰고 hole들을 다른 한 곳으로 몰아 큰 block을 만드는 것 (디스크 조각 모음 같은)
>>- 매우 비용이 많이 드는 방법임 (전체 프로그램의 바인딩과 관련된 문제이기 때문에)
>>- 최소한의 메모리 이동으로 compaction 하는 방법이 효율적임 (매우 복잡한 문제)
>>- compaction 은 프로세스의 주소가 실행 시간에 동적으로 재배치 가능한 경우에만 수행될 수 있음 (런타임 바인딩이 지원되는 경우에만)

<br>

[목차로 이동](#목차)

>### Noncontiguous allocation (불연속 할당)
>- Paging
>>- Paging
>>>- Process 의 virtual memory 를 동일한 사이즈의 page 단위로 나눔
>>>- Virtual memory 의 내용이 page 단위로 noncontiguous 하게 저장됨
>>>- 일부는 backing storage 에, 일부는 physical memory 에 저장
>>- Basic Method
>>>- physical memory 를 동일한 크기의 frame 으로 나눔
>>>- logical memory 를 동일 크기의 page 로 나눔 (frame 과 같은 크기)
>>>- 모든 가용 frame 들을 관리
>>>- page table 을 사용하여 logical address 를 physical address 로 변환
>>>- External fragmentation 발생 안함
>>>- Internal fragmentation 발생 가능
>- Implementation of Page Table (페이지 테이블 구현)
>>- Page table 은 main memory 에 상주
>>- Page table base register (PTBR) 가 page table 을 가리킴
>>- Page table length register (PTLR) 가 테이블 크기를 보관
>>- 모든 메모리 접근 연산에는 2번의 memory access 필요
>>- page table 접근 1번, 실제 data/instruction 접근 1번
>>- 속도 향상을 위해 별도의 하드웨어로써 CPU와 메인메모리 계층 사이에 있는 associative register 혹은 translation look-aside buffer (TLB) 라 불리는 고속의 lookup hardware cache 사용
>>>- TLB 를 통해 접근해보고 캐시미스가 났을때 page table로 접근하여 물리 주소를 알아냄
>- Associative Register
>>- Associative Register (TLB) : parallele search 가 가능
>>>- TLB 에는 page table 중 일부만 존재
>>- Address translation
>>- page table 중 일부가 associative register 에 보관되어 있음
>>- 만약 해당 page # 가 associative register 에 있는 경우 곧바로 frame # 을 얻음
>>- 없는 경우 main memory 에 있는 page table 로 부터 frame # 을 얻음
>>- TLB 는 context switch 때 flush (remove old entries)
>>>- 프로세스마다 TLB 와 page table 이 다르기 때문임
>- Two Level Page Table
>>- 현대의 컴퓨터는 address space 가 매우 큰 프로그램 지원
>>>- 32 bit address 사용시 2^32 (4G) 의 주소 공간
>>>>- page size 가 4K 일 경우 1M 개의 page table entry 필요
>>>>- 각 page entry 가 4B 일 경우 프로세스당 4M 의 page table 필요
>>>>- 그러나, 대부분의 프로그램은 4G 의 주소 공간 중 지극히 일부분만 사용하므로 page table 공간이 심하게 낭비됨
>>- page table 자체를 page 로 구성
>>- 사용되지 않는 주소 공간에 대한 outer page table 의 엔트리 값은 NULL (대응하는 inner page table 이 없음을 의미)
>- Two Level Page Table Example
>>- logical address (on 32 bit machine with 4K page size) 의 구성
>>>- 20 bit 의 page number
>>>- 12 bit 의 page offset
>>- page table 자체가 page 로 구성되기 때문에 page number 는 다음과 같이 나뉨 (각 page table entry 가 4B)
>>>- 10 bit 의 page number
>>>- 10 bit 의 page offset
>>```
>># logical address
>>-----------------------------
>>| page number | page offset |
>>|  p1  |  p2  |      d      |
>>|  10  |  10  |      12     |
>>-----------------------------
>>```
>>- p1 은 outer page table 의 index
>>- p2 는 outer page table 의 page 에서의 변위 (displacement)
>>- 비트수 결정
>>>- 4k page size 는 10^12 크기이므로 page offset 은 12bit 필요
>>>- inner page table 이 4K 크기인데 각 entry 가 4B 이기때문에 entry 수가 1K 이므로 p2 는 10비트가 필요
>>>- 남은 비트를 p1 에 할당
>>- 64 bit machine with 4K page size 인 경우
>>>- page size 가 4K 로 고정이므로 page offset 은 12bit
>>>- 4K 안에 8B 짜리 엔트리를 2^9 개 담을 수 있으므로 p2 는 9bit
>>>- p1은 남은 43bit가 할당됨
>>- 페이지 테이블이 엔트리가 100만개 이상이 필요하고 2단계 페이징을 쓰더라도 여전히 100만개 이상이 필요하며 바깥쪽 테이블이 생기기 때문에 오히려 시간도 손해 공간도 손해인거 같은데 왜 쓰는가
>>>- 실제 공간중 상당 부분은 사용되지 않음
>>>>- 페이지 테이블을 만들때는 사용되지 않는 공간들도 엔트리를 안만들 수 없음
>>>>- 2단계 페이징을 쓰면 바깥쪽 테이블은 전체 논리 메모리 크기만큼 만들어지지만 사용되지 않는 주소에 대해 안쪽 테이블은 만들어지지 않고 바깥쪽 테이블에선 NULL 이므로 메모리 공간에 매우 효과적임
>- 예상 질문
>>- 내부 단편화와 외부 단편화란?
>>>- 내부 단편화는 프로세스가 필요한 양보다 더 큰 메모리 공간에 할당되어 메모리 낭비가 발생하는 것임
>>>- 외부 단편화는 메모리에 프로세스가 할당될 만큼 충분한 공간이 존재하지만 공간이 쪼개어져서 프로세스를 넣을 수 없는 것을 의미함
>>- 페이징의 장점과 단점은?
>>>- 장점 : 페이징 기법을 사용하게 되면 하나의 프로세스가 사용하는 메모리 공간이 연속적이어야 한다는 제약을 없애 외부 단편화 문제점을 해결할 수 있음
>>>- 단점 : Mapping 과정 또한 늘어나기 때문에 시간적인 손해가 발생할 수 있고 페이지 단위에 알맞게 꽉채워 쓰는게 아니므로 내부 단편화 문제는 해결되지 않음

<br>

[목차로 이동](#목차)