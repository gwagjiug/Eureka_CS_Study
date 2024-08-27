# 1.1 운영체제

## 1.1.1. 운영체제란?

- 하드웨어 계층과 소프트웨어 계층을 연결하는 소프트웨어 계층
- 컴퓨터 시스템의 한정된 자원을 관리하고, 사용자가 컴퓨터를 사용할 수 있는 환경을 제공

## **1.1.2 운영체제의 목적**

- 한정된 자원 안에서 정확하고 빠르게 주어진 문제를 해결하는 것이 목적
- OS 성능 평가 기준
    - 처리 능력 향상
    - 반환 시간 단축
    - 사용 가능도 향상
    - 신뢰도 향상

## 1.1.3 CPU와 메모리 구조

- **CPU (= 프로세서): 프로그램 실행 시 연산 수행**
- **메모리: 데이터를 저장하기 위한 기억장치**
    - 주 기억장치(메인 메모리: RAM) + 보조 기억장치(SSD, HDD)

> 메모리의 계층구조
> 

<img width="354" alt="Untitled" src="https://github.com/user-attachments/assets/4aee6cae-5168-4f14-9ee9-2fbb30d87d1d">


- **레지스터**: CPU 내부에 존재하며 필요한 데이터를 임시로 저장하는 기억장치. (다음 프로그램 실행을 위한 주소 기억)
- **캐시 메모리**: CPU와 RAM 사이의 속도 차이를 해결하기 위한 기억장치
- **RAM:** 프로그램을 실행할 때 필요한 정보를 저장. 휘발성 기억장치. 주기억장치
- **하드디스크**: 데이터와 프로그램 저장. 비휘발성 기억장치. 보조기억장치

<aside>
💡 CPU는 하나의 프로세스만 처리할 수 있어서 멀티 프로세스 환경에서는 OS가 스케줄링을 통해 CPU에 프로세스를 할당한다 !

</aside>

## 1.1.4 커널과 시스템 콜

시스템 소프트웨어를 구성하는 요소 

- **커널 모드**- 메모리, 스케줄링 작업 등을 담당하는데, 하드웨어에 직접 접근해 구동한다. 메모리, CPU와 같은 자원을 사용
- **사용자 모드**- 커널 모드의 자원에 접근할 수 없게 제한(분리시켜 둠). 시스템 콜을 통해서 자동적으로 프로세스를 관리하여 사용자 모드에서 계산된다.

### **시스템 콜**

- 커널 모드와 사용자 모드 간에 필요한 기능을 수행하기 위해 프로세스에서 호출하는 함수
- 실행된 프로세스가 자원에 접근하기 위해 시스템 콜을 호출하여 커널에 요청한다.

---

# 1.2 프로세스

## 1.2.1 프로세스와 스레드

### **프로세스**

**컴퓨터에서 실행중인 하나의 프로그램**

> 프로세스의 메모리 영역 구조
> 

프로세스는 OS로부터 독립된 메모리 영역(코드, 데이터, 스택, 힙)을 할당받으며, 다른 프로세스의 메모리 영역에 접근할 수 없다.

- 스택 - **지역변수**, 함수의 매개변수, 반환되는 주소값
- 힙 - 사용자 동적 할당
- 데이터 - **전역변수**, 정적변수, 배열, 구조체
- 코드 - 실행할 **코드**(기계어)

`스택 오버플로: 스택 영역이 힙 영역을 침범하는 경우 ex) 과도한 재귀호출`  

`힙 오버플로: 힙 영역이 스택 영역을 침범하는 경우 ex) 과도한 메모리 동적 할당` 

### **스레드**

**프로세스에서 실제로 실행되는 흐름의 단위**

- 각 스레드는 스택 영역을 할당받음
- 스택을 제외한 영역은 다른 스레드와 공유
![이미지 2024  7  13  오후 4 35](https://github.com/user-attachments/assets/58989cf3-b775-4598-a501-02114a94469b)


## 1.2.2 PCB

**PCB**(Process Control Block): 프로세스에 대한 모든 정보를 가지고 있는 블록 

- PID: 프로세스 고유 번호
- PC: 다음으로 실행될 주소값
- 프로세스 상태 정보
- CPU 레지스터에 저장되는 정보

## 1.2.3 프로세스의 생성

다음 프로세스 실행을 위한 복사 작업이다.

새로운 프로세스(자식)는 기존 프로세스(부모)에서 **fork()** 함수를 호출해 생성 **(부모 프로세스는 자식 프로세스의 PID 값을, 자식 프로세스는 0을 반환)**
<img width="338" alt="Untitled" src="https://github.com/user-attachments/assets/e8c84b28-bf15-4d70-b455-7079092417b7">



## 1.2.4 프로세스 상태도

모든 프로세스는 CPU에 의해 생성되고 소멸하는 과정을 거친다.

> 프로세스 상태도
> 
<img width="544" alt="Untitled" src="https://github.com/user-attachments/assets/8e6b6edf-f576-45c0-a040-7adf44410eec">


- **생성**: 프로세스가 PCB를 가지고 있지만 OS로부터 승인받기 전
- **준비**: OS로부터 승인받은 후 준비 큐에서 CPU 할당을 기다림
- **실행**: 프로세스가 CPU를 할당받아 실행함
- **대기**: 프로세스가 입출력이나 이벤트 발생을 기다려야 해서 CPU 사용을 멈추고 기다림
- **종료**: 프로세스 실행을 종료함

## 1.2.5 멀티 프로세스와 멀티 스레드

- **동시성**: 하나의 코어에서 **여러 작업을 번갈아 가면서 처리하는 방식** → 하나의 CPU에서 여러 작업 처리하기 위해 콘텍스트 스위칭이 일어나 오버헤드 발생
- **병렬성**: **여러개의 CPU에서 각 작업을 동시에 처리**하는 방식

> 멀티 프로세스 실행 시
> 
- 안정적이지만 시간과 메모리 공간을 많이 사용한다는 단점 존재
- 콘텍스트 스위칭 작업이 이뤄져야 함
- 오버헤드 발생
- **IPC**를 통해 독립된 프로세스 간 자원 공유를 해야한다. (공유할 메모리를 직접 참조하는 것보다 비효율적)

> 멀티 스레드 실행 시
> 
- 스레드 간에 힙, 데이터, 코드 영역 공유
- 콘텍스트 스위칭할 때 오버헤드가 적게 발생하고 IPC를 사용하지 않아도 되어 멀티 프로세스의 단점 보완
- 스택 영역을 다른 스레드와 **함께 사용하므로 공유 자원에 대한 동기화** 필수!!
- 스레드에 문제가 생기면 프로세스 내 다른 스레드에 영향을 미칠 수 있다.

<aside>
💡 멀티 프로세스와 멀티 스레드의 중요한 차이점 중 하나는 자원 공간의 공유 여부이다.

</aside>

## 1.2.6 콘텍스트 스위칭

- 멀티 프로세스 환경에서 CPU가 다른 프로세스를 처리하기 위해 처리중인 프로세스를 변경하는 것
- 처리하던 프로세스 정보를 저장하고, 다음으로 처리할 프로세스 정보를 레지스터에 로드하면서 오버헤드 발생

<aside>
💡 콘텍스트 스위칭 시 오버헤드 발생량: 멀티 프로세스>>>멀티 스레드
멀티 프로세스는 고유한 메모리 영역을 갖는 반면, 멀티 스레드는 스택 영역을 제외한 메모리 영역을 공유하기 때문에 레지스터에 로드해야 하는 데이터 양이 적어서 오버헤드가 적게 발생함.

</aside>

## 1.2.7 프로세스 동기화

프로세스 동기화는 **멀티 프로세스 환경에서 공유 자원의 일관성을 보장**하기 위한 방법이다.

- 경쟁 상태로 인한 예기치 못한 결과를 초래하는 것(임계 영역)을 방지하기 위해 프로세스 동기화가 이뤄져야 한다.
    - 경쟁 상태: 공유 자원에 동시에 접근해 경쟁하는 상태
    - ex) 멀티 스레드 환경에서 두 개 이상의 스레드가 공통의 전역변수에 접근할 경우 등

> 경쟁상태가 발생하는 것을 방지하기 위한 조건
> 
- **상호배제 기법**: 어떤 프로세스가 임계영역을 실행 중일 때 다른 프로세스가 임계영역에 접근할 수 없다.
    - ex) **뮤텍스, 세마포어** 기법
- 진행: 임계 영역을 실행 중인 프로세스가 없을 때 다른 프로세스가 임계 영역을 실행한다.
- 한정된 대기: 임계 영역에 접근을 요청했을 때 무한한 시간을 기다리지 않는다.

<aside>
💡 작업 처리와 관련된 용어

</aside>

| 작업 순서 | 작업을 위한 대기 |
| --- | --- |
| 동기: 여러 작업을 처리할 때 작업 순서를 보장함  | 블로킹: 작업을 수행할 때 대기할 수있다는 것을 의미하며 작업 순서를 보장하지 않음 |
| 비동기: 여러 작업을 처리할 때 순서를 보장하지 않음 | 넌블로킹: 작업을 시작하면 대기 없이 수행한다는 것을 의미 |

## 1.2.8 교착 상태

**교착 상태**: 상호배제 기법 때문에 2개 이상의 프로세스가 각각 자원을 가지고 있으면서 서로의 자원을 요구하며 기다리는 상태

> 교착상태 발생 조건 → 한가지를 제거하면 교착상태가 발생하지 않음
> 
1. **상호배제**
2. **점유와 대기**
3. **비선점**
4. **환형 대기**

> 교착상태 해결방법
> 
- 예방
- 회피
- 발견
- 복구

## 1.2.10 IPC

**IPC(Inter Process Communication)**: 프로세스 간에 자원을 공유하는 방식

> IPC 종류
> 
- 공유 메모리
- 소켓: 네트워크 소켓을 이용하는 통신으로 외부 시스템과도 이용할 수 있다.
- 세마포어: 접근하는 프로세스를 제어해 공유 자원 관리
- 파이프: FIFO 형태의 메모리인 파이프 이용(단방향 통신만 지원)
- 메시지 큐: FIFO 형태의 큐 자료구조 이용하여 프로세스 간 메시지를 주고받는 방식

---

# 1.3 스케줄링

## 1.3.1 스케줄링의 목적

스케줄링의 주된 목적은 **멀티 프로세스 환경에서 모든 프로세스를 공평하게 실행하는 것**

- 공평성, 효율성, 안정성, 반응 시간 보장, 무한 연기 방지
- **프로세스 스케줄링**은 CPU를 사용하려고 하는 프로세스들 사이의 **우선순위**를 관리하는 작업이다.

## 1.3.2 스케줄링의 단계
<img width="658" alt="Untitled" src="https://github.com/user-attachments/assets/1ea7d59b-c5e3-446f-ab41-8cc02b03d171">


- **장기 스케줄링** - 어떤 프로세스를 준비 큐에 넣을지 결정함
- **중기 스케줄링** - 메모리에 로드된 프로세스 수 관리
- **단기 스케줄링** - 준비 큐에 있는 대기 상태 프로세스 중 어떤 프로세스를 실행할지 **스케줄링 알고리즘**으로 결정

## 1.3.3 스케줄링 알고리즘

단기 스케줄러가 준비 큐에 있는 프로세스 중 어떤 프로세스를 실행시킬지 결정하는 데 사용 

> 평가기준
> 
- CPU 사용률
- 처리량
- 응답 시간
- 반환 시간
- 대기시간

### 비선점형 스케줄링

실행중인 프로세스가 종료될 때까지 다른 프로세스를 실행할 수 없다.

> 알고리즘
> 
- **FCFS(First Come First Served**): 먼저 들어온 프로세스가 우선순위를 가짐
- **SJF(Shortest Job First)**: 실행시간이 짧은 프로세스가 우선순위를 가짐
- **HRRN(Hightest Response Ratio Next)**
    - 대기중인 프로세스 중 현재 응답률이 가장 높은 것을 선택하는 기법
    - SJF의 약점인 기아 현상을 보완한 기법으로 긴 작업과 짧은 작업 간의 불평등 완화
        - 기아현상: 시스템 부하가 많아서 준비 큐에 있는 짧은 작업의 프로세스가 무한정 기다리는 현상
        - 짧은 작업의 우선순위를 높여주도록 하는 에이징(Aging) 활용

### 선점형 스케줄링

스케줄러가 실행중인 프로세스를 중단시키고 다른 프로세스를 실행할 수 있다.

> 알고리즘
> 
- **RR(Round Robin)**
    - 프로세스는 같은 크기의 CPU 시간을 할당
    - 프로세스는 순서대로 일정 시간 동안 실행되며, 일정 시간을 초과하면 다른 프로세스를 실행
    - 실시간에 많이 사용됨. 시분할 시스템 사용
- **SRTF(Shortest Remaining Time First)**
    - 준비 큐에서 대기 시간이 가장 짧게 남은 프로세스를 우선 수행
    - 남은 처리시간이 더 짧다고 판단되는 프로세스가 준비큐에 생기면 그 프로세스가 선점된다
        - 비선점 방식의 스케줄링 기법에 선점 방식을 도입한 기법
- 멀티 레벨 스케줄링: 준비 큐를 목적에 따라 여러 개로 분리해 사용

> SJF와 SRTF 기법 비교
> 

|  | 개념 | 장점 | 단점 |
| --- | --- | --- | --- |
| SJF | 프로세스가 도착하는 시점에 따라 그 당시 가장 작은 서비스 시간을 갖는 프로세스가 종료 시까지 자원 점유 | - 일괄처리 환경에서 구현이 용이함 | - 작업 시간이 적은 프로세스가 계속 들어오는 경우, 기존 작업 시간이 긴 프로세스는 기아 현상 발생
- SRT에 비해 평균 대기 시간이 김 |
| SRTF | 가장 짧은 시간이 소요되는 프로세스를 먼저 수행. 남은 처리 시간이 더 짧다고 판단되는 프로세스가 준비큐에 생기면 언제라도 프로세스가 선점됨 | - 시분할 시스템에 활용 시 유용
- SJF에 비해 평균 대기 시간이나 반환시간이 짧음 | - 준비큐에 있는 각 프로세스의 서비스 시간을 지속적으로 추적해야 하므로 오버헤드 증가 |

---

# 1.4 메모리 관리 전략

## 1.4.1 논리 메모리와 물리 메모리

논리 메모리 영역(가상 메모리 영역): 프로세스가 보는 메모리 영역

물리 메모리 영역: 실제 사용되는 메모리 영역(RAM)

논리주소(가상주소): CPU가 프로세스를 실행하며 보는 주소값

물리주소: 실제 메모리가 사용되는 주소

**메모리 관리  장치(MMU)**: 논리주소 → 물리주소로 변환하는 하드웨어 장치

## 1.4.2 연속 메모리 할당

연속 메모리 할당은 멀티 프로세스 환경에서 여러 프로세스를 메모리에 연속적으로 로드하는 방법

- 주기억장치 공간 내에서 인접되게 연속하여 저장
- p1 / p2 / p3 / p4 순서대로 5MB으로 하나씩 분할

### 고정 분할 방식

메모리 영역을 분할한 뒤 각 영역에 프로세스를 할당하는 방식

- 메모리에 올릴 수 있는 프로세스 수와 각 프로세스 크기가 제한된다는 단점
- 단편화 발생 가능

### 가변 분할 방식

할당할 프로세스의 크기에 따라 메모리 공간을 분할하는 방식

> 메모리 할당 알고리즘
> 
- 최초 적합: 가용 메모리 공간에서 프로세스 크기만큼 비어있는 공간을 찾아 차례대로 프로세스 로드
- 최적 적합: 할당하려는 프로세스 크기 이상인 가용 메모리 공간 중에서 가장 작은 공간에 프로세스 할당
- 최악 적합: 할당하려는 프로세스 크기보다 큰 가용 메모리 공간 중 가장 큰 공간에 프로세스 할당

## 1.4.3 비연속 메모리 할당

프로세스의 메모리 영역을 나눠서 메모리 공간에 저장

- 주로 가상기억장치에서 사용
- 분산된 페이지를 테이블에 넣어 연속적으로 쓰기 위해 논리적으로 공간을 만든다.
- 연속 메모리 할당을 하여 생길 수 있는 메모리 공간의 공백을 비연속적으로 할당하므로써 메모리 공간을 효율적으로 사용할 수 있다.

### 페이징

- 프로세스의 논리 메모리 영역과 물리 메모리 영역을  각각 **일정한 크기로 분할**하여 주기억장치 공간 내 분산하여 배치 → **고정적**으로 분할
- 메모리에 연속적으로 할당할 필요가 없어 외부 단편화 문제 해결 가능
- 하지만 프로세스의 마지막이 메모리보다 작을 수 있으므로 **내부 단편화 발생 가능**
    - 내부 단편화 해결 방안
        - Slab Allocator: 페이지 프레임을 할당받아 공간을 작은 크기로 분할하고 메모리 요청 시 작은 크기로 메모리를 할당/해제하는 동적 메모리 관리 기법
        - 통합, 압축

### 세그먼테이션

- 프로세스의 메모리 영역을 **논리적 단위(스택, 힙)인 세그먼트로 분할**해 메모리를 할당 → **가변적**으로 분할
- 논리적 단위로 나눠 저장하므로 단위별 데이터를 보호하기 쉬움
- **세그먼트 크기가 균등하지 않아 외부 단편화 발생 가능**
    - 외부 단편화 해결 방안
        - 버디 메모리 할당: 요청한 프로세스 크기에 가장 알맞은 크기를 할당하기 위해 메모리를 2n크기로 분할하여 메모리를 할당하는 기법
        - 통합, 압축

---

# 1.5 가상 메모리

## 1.5.1 가상 메모리란

가상 메모리는 프로세스의 일부만 메모리에 로드하고, 나머지는 디스크에 둔상태로 프로세스를 실행하는 방식

- 프로그램이 메모리 크기에 대한 제약을 덜 받을 수 있다.
- 동시에 많은 프로그램을 실행하므로 CPU 이용률과 처리율을 높일 수 있다.
- 필요한 영역만 메모리에 로드해 스와핑 횟수를 줄여서 프로그램 실행 속도를 높일 수 있다.

## 1.5.2 요구 페이징

요구 페이징은 프로세스에서 필요한 페이지만 메모리에 로드하는 방식

- 페이지를 모두 메모리에 로드하지 않고 초기에 필요한 영역만 로드한 후 다른 영역은 요청이 올 때 메모리에 로드
- 필요한 페이지를 물리 메모리에 로드하고, 필요하지 않은 페이지는 디스크에 저장

## 1.5.3 스레싱

- 스레싱은 동시에 일정 수 이상의 프로그램을 실행했을 때 CPU 이용률이 떨어지는 상황을 말한다.
- 어떤 프로세스가 계속적으로 페이지 부재가 발생하여 프로세스가 실제 처리 시간보다 페이지 교체 시간이 더 많아지는 현상
    - ex) 다중 프로그래밍 정도가 일정 수준 이상 높아질 때
- 오류율이 클수록 스레싱이 많이 발생한 것이고, 스레싱으로 인해 전체 시스템의 성능 및 처리율은 저하된다.

<aside>
💡 페이징 시간 > CPU 처리 시간 → CPU 뒤에 프로세스가 쌓여 처리를 못하게 되어서 CPU 과부하

</aside>

<br />


> 스레싱 예방하는법
> 
- 준비큐
- 페이지 교체 알고리즘
    - 워킹 세트: 자주 사용하는 페이지 저장하여 빈번한 페이지 교체 현상을 줄이고자 하는 기법
    - FIFO 페이지 교체: 가장 먼저 들어온 페이지부터 교체
    - LRU 페이지 교체: 가장 오랫동안 사용되지 않은 페이지 교체
    - LFU 페이지 교체: 페이지 참조횟수가 가장 적은 페이지 교체

<aside>
💡 결국에는 스레싱을 최소화하기 위한 상황에 맞는 스케줄링을 해야된다.

</aside>

---

# 1.6 캐시 메모리

## 1.6.1 캐시 메모리와 지역성

캐시 메모리는 CPU와 메인 메모리 간에 데이터 접근 시 속도 차이를 줄이기 위해 사용한다.

- CPU에서 메인 메모리에 있는 데이터를 가져올 때 자주 사용하는 데이터는 캐시 메모리에 따로 저장 → 메인 메모리에 접근하는 것보다 속도 향상
- **지역성**을 활용하여 캐시메모리에 데이터 저장 → **효율성의 극대화**

### 지역성

지역성은 프로세스가 실행되는 동안 주기억장치를 참조할 때 일부 페이지만 집중적으로 참조하는 특성이다.

프로세스가 집중적으로 사용하는 페이지를 알아내는 방법의 하나로, 가상기억장치 관리의 이론적 근거가 되었다.

스레싱을 방지하기 위한 워킹 셋 이론의 기반 

> 지역성 유형
> 
- 시간 지역성: 최근 참조했던 메모리는 다시 참조될 가능성이 높은 특성
- 공간 지역성: 실제 참조한 메모리 근처의 메모리를 참조
- 순차 지역성: 명령어가 순차적으로 구성된 특성