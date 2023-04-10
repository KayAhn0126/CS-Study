# Process Synchronization

## 🍎 프로세스 동기화
- 프로세스 동기화란 프로세스들 사이의 수행 시기를 맞추는것을 의미한다.
- 프로세스들 사이의 수행 시기를 맞추는 것은 크게 아래 두가지를 의미한다.
    - 실행 순서 제어: 프로세스를 올바른 순서대로 실행하기
    - 상호 배제: 동시에 접근해서 안되는 자원에 하나의 프로세스만 접근하게 하기

## 🍎 Race Condition
- Race Condition이 생길 수 있는 예시를 보자
### 📖 Interrupt handler vs kernel
![](https://i.imgur.com/zwq3bus.png)
- 현재 커널에서 Count라는 변수의 값을 1 증가 시키는 작업이 실행되고 있다.
- 보통 고급언어에서 변수의 값을 1 증가 시키는 문장이 CPU 내부에서는 이미지의 1,2,3번 처럼 여러개의 instruction을 통해서 실행이 된다.
    - 1. Load
    - 2. Inc
    - 3. Store
- CPU에서는 "Count++"을 어떻게 실행하냐면,
    - 먼저 memory에 있는 변수 Count의 값을 CPU안에 있는 레지스터로 불러들인다.(load)
    - 그 레지스터 값을 1 증가 시키고 (Inc)
    - 이후 가져왔던 변수 Count에 증가 시킨 값을 저장한다.
- 하지만 만약 CPU가 Count 변수의 값을 레지스터로 읽어들인 상태(Load)에서 이미지의 Interrupt handler가 실행이 된다면 온전히 현재 일을 끝내지 못한 상태에서 Count-- 작업을 실행하게 된다.
- Count-- 작업도 아래와 같은 instruction으로 이루어져있다.
    - 1. Load
    - 2. Dec
    - 3. Store
- 전체적인 그림에서 보자면 아래와 같은 프로세스로 진행이 되어서 결과적으로는 Count-- 작업은 실행이 안된것과 마찬가지인 결과가 나온다.
    - Count++ 작업으로 인한 Count 값을 레지스터A에 Load
    - 이때 Count-- 작업으로 인한 Count값을 또 다른 레지스터 B에 Load
    - 레지스터 B에서 값을 1만큼 빼기
    - 레지스터B에 있는 값을 Count 변수에 저장
    - 아까 실행중이던 Count++ 작업으로 돌아와서 레지스터A에 있는 값 1만큼 더하기
    - 레지스터A에 있는 값을 Count 변수에 저장
    - 끝
- 이 경우, 결과적으로는 Count-- 한것은 반영이 안된다.
- 결국에는 실행 순서를 정해주면 된다.
- 어떠한 프로세스가 공유 자원에 대해서 일을하고 있을때 다른 프로세스가 해당 공유 자원에 접근하지 못하게 막으면된다.
- 하지만 무조건 막는다고 좋은것은 아니다!

## 🍎 Process Synchronization 문제
- 공유데이터의 동시 접근은 데이터의 불일치 문제를 발생시킬 수 있다.
- 일관성 유지를 위해서는 협력 프로세스간의 실행순서를 정해주는 매커니즘 필요
- Race Condition
    - 여러 프로세스들이 동시에 공유 데이터를 접근하는 상황
    - 데이터의 최종 연산 결과는 마지막에 그 데이터를 다룬 프로세스에 따라 달라짐

## 🍎 The Critical Section Problem
- n개의 프로세스가 공유 데이터를 동시에 사용하기 원하는 경우
- 각 프로세스의 code segment에는 공유 데이터를 접근하는 코드인 critical section이 존재
- problem
    - 하나의 프로세스가 Critical section에 있을 때 다른 모든 프로세스는 critical section에 들어갈 수 없어야 한다.

![](https://i.imgur.com/NmaRDM2.png)
- 이미지에서 빨간 네모 박스가 critical section이다.
