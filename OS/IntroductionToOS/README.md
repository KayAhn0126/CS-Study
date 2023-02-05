# Introduction to OS

## 🍎 운영체제란?
- 하드웨어 바로 위에 설치되어 소프트웨어와 하드웨어를 연결하는 소프트웨어 계층
- 작은의미에서의 운영체제는 커널
    - 전공자 입장에서의 운영체제는 보통 커널을 뜻한다.
- 큰 의미에서의 운영체제는 커널을 포함한 애플리케이션, 유틸리티이다.

### 📖 커널이란?
- 운영체제의 핵심 부분으로 항상 메모리에 상주한다.
- Process 관리, Memory 관리, Device 관리를 한다.

## 🍎 운영체제의 목적
- 컴퓨터 시스템의 자원을 효율적으로 관리하는것이 **운영체제의 가장 중요한 임무!**
    - 위의 커널이 하는 임무와 같다!

## 🍎 운영체제의 분류
- 동시 작업 가능 여부
    - 단일 작업
        - 이전의 운영체제
        - 한번에 하나의 작업만 수행가능
    - 다중 작업
        - 현대의 운영체제
        - 동시에 두개 이상의 작업 수행가능
- 사용자의 수
    - 단일 사용자
        - 일반 컴퓨터
    - 다중 사용자
        - 서버
- 처리방식
    - **일괄 처리 (batch processing)**
        - 작업 요청을 일정량 모아서 한꺼번에 처리한다
        - 작업이 완전 종료될 때 까지 기다려야한다.
        - 현대 운영체제에서는 찾아보기 힘든 방식
    - 일괄 처리 방식에 대한 예시
        - 예전에는 동사무소에서 주민등록등본을 요구하면 30분 기다리라고 한다.
        - 직원이 30분동안 주민등록등본을 요구한 사람들의 신분증을 모아 창고로 들어가 필요한 서류를 한번에 가져왔다고 한다.
        - 그 당시에는 지금처럼의 시스템이 없었기 때문이었고 직원이 매번 왔다갔다 할 수 없으니 신분증을 일정량 모아 한번에 처리하는 방식인 일괄 처리 방식을 사용했다고 한다.
    - **시분할 (time sharing)**
        - 여러작업 수행시 컴퓨터 처리능력을 일정한 시간 단위로 분할하여 사용
        - 일괄처리(batch) 시스템에 비해 짧은 응답 시간을 가진다.
        - CPU가 각각의 프로그램에 아주 짧은 시간 동안 CPU사용 권한을 준다고 생각하면 된다.
    - **실시간 (Realtime OS)**
        - 정해진 시간 안에 어떠한 일이 반드시 종료됨이 보장 되어야 하는 실시간 시스템들을 위한 OS
        - dead line이 있다.
        - 예) 원자로, 미사일 제어, 반도체 장비, 로보트 제어
    - 시분할과 실시간은 비슷해 보이는데 무슨 차이가 있을까?
        - 일반적으로 시분할 처리 방식은 일반 컴퓨터에서 사용하고 하나의 CPU로 여러 프로그램에 통제권을 아주 빠르게 번갈아가며 실행한다, 하지만 실시간 처리 방식은 특정 목적을 가지는 시스템에서 사용하는만큼 하나의 CPU 통제권을 가지고 임무를 수행한다.

## 🍎 운영체제의 예
- UNIX
    - 대형 컴퓨터를 위한 OS
    - C언어로 작성
    - 높은 이식성
- Windows
    - 일반 컴퓨터를 위한 OS
    - GUI 기반
    - Plug and Play

## 🍎 운영체제의 구조
- CPU -> 누구한테 CPU를 줄까?
    - CPU 스케쥴링
- Memory -> 한정된 메모리를 어떻게 잘 관리하지?
    - 메모리 관리
- Disk -> 디스크에 파일은 어떻게 접근/보관하지?
    - 파일 관리
    - 엘레베이터 로직
        - 디스크는 헤드가 이동하면서 처리할게 있으면 처리하고 간다.
- I.O devices -> 컴퓨터와 어떻게 정보를 주고 받게 하지?
    - 인터럽트
        - CPU가 쉴 새 없이 일할때 평소에는 I.O 기기에 관여를 안한다.
        - 하지만 I.O 기기에서 Interupt 신호를 CPU에게 보고하면 다음 작업을 수행하기 전에 I.O에서 들어온 작업을 처리하고 간다.