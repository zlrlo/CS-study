# OS

## 프로그램과 프로세스

- 프로그램은 저장장치에 저장되어 있는 정적인 상태이고, **프로세스는 실행을 위해 메모리에 올라온 동적인 상태**이다.

## 스레드

- CPU 스케줄러가 `CPU에 전달하는 일 하나`를 스레드라고 하며, 하나의 프로세스에는 여러개의 스레드가 존재하기도 한다.

### 스레드 관련 용어

- **멀티스레드**
  - 프로세스 내 작업을 여러 개의 스레드로 분할함으로써 작업의 부담을 줄이는 프로세스 운영 기법이다.
- **멀티태스킹**
  - 운영체제가 CPU에 작업을 줄 때 시간을 잘게 나누어 배분하는 기법이다.
- **멀티프로세싱**
  - CPU를 여러 개 사용하여 여러 개의 스레드를 동시에 처리하는 작업 환경을 말한다.
- **CPU 멀티스레드**
  - 한 번에 하나씩 처리해야 하는 스레드를 파이프라인 기법을 이용하여 동시에 여러 스레드를 처리하도록 만든 병렬 처리 기법이다.

### 멀티스레드의 장점

- **응답성 향상**
  - 한 스레드가 입출력으로 인해 작업이 진행되지 않더라도 다른 스레드가 작업을 계속하여 사용자의 작업 요구에 빨리 응답할 수 있다.
- **자원 공유**
  - 한 프로세스 내에서 독립적인 스레드를 생성하면 프로세스가 가진 자원을 모든 스레드가 공유하게 되어 작업을 원활하게 진행 할 수 있다.
- **효율성 향상**
  - 불필요한 자원의 중복을 막음으로써 시스템의 효율이 향상된다.
- **다중 CPU 지원**
  - 2개 이상의 CPU를 가진 컴퓨터에서 멀티스레드를 사용하면 다중 CPU가 멀티스레드를 동시에 처리하여 CPU 사용량이 증가하고 프로세스의 처리 시간이 단축된다.

## 프로세스 제어 블록(Process Control Block, PCB)

- 프로세스를 실행하는데 필요한 중요한 정보를 보관하는 자료 구조로, 모든 프로세스는 고유의 프로세스 제어 블록을 가진다.
- 프로세스 제어 블록은 프로세스 생성 시 만들어져서 프로세스가 실행을 완료하면 폐기된다.

## 라운드 로빈 스케줄링

- 순환 순서 방식으로 번역되는 라운드 로빈 스케줄링은 한 프로세스가 할당받은 시간동안 작업을 하다가 작업을 완료하지 못하면 준비 큐의 맨 뒤로 가서 자기 차례를 기다리는 방식이다.
- 선점형 알고리즘 중 가장 단순하고 대표적인 방식으로, 프로세스들이 작업을 완료할 때까지 계속 순환하면서 실행된다.
- 라운드 로빈 스케줄링은 FCFS 스케줄링과 유사한데, 차이점은 각 프로세스마다 CPU를 사용할 수 있는 최대 시간, 즉 **타임 슬라이스**가 있다는 것이다.

#### 단점

- 라운드 로빈 스케줄링과 FCFS 스케줄링의 평균 대기 시간이 같다면 라운드 로빈 스케줄링이 더 비효율적인 알고리즘이다.
- 라운드 로빈 스케줄링 같은 선점형 방식에서는 **문맥 교환 시간**이 추가되기 때문이다.
- 결론적으로 타임 슬라이스는 되도록 작게 설정하되 문맥 교환에 걸리는 시간을 고려하여 적당한 크기로 설정하는 것이 중요하다.

## SRT 우선 스켈줄링

- SRT(Shortest Remaining Time)
- SRT 스케줄링은 기본적으로 **라운드 로빈 스케줄링**을 사용하지만, CPU를 할당받을 프로세스를 선택할 때 남아있는 작업 시간이 가장 적은 프로세스를 선택한다.
- 라운드 로빈 스케줄링이 큐에 있는 순서대로 CPU를 할당한다면, SRT 스케줄링은 남은 시간이 적은 프로세스에 CPU를 먼저 할당한다.

#### 단점

- 현재 실행 중인 프로세스와 큐에 있는 프로세스의 남은 시간을 주기적으로 계산하고, 남은 시간이 더 적은 프로세스와 문맥 교환을 해야 하므로 SJF 스케줄링에는 없는 작업이 추가된다.
- 운영체제가 프로세스의 종료 시간을 예측하기 어렵고 **아사 현상**이 일어아기 때문에 잘 사용하지 않는다.

💡 참고 서적: 쉽게 배우는 운영체제