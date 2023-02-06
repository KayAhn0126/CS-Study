# System Structure & Program Execution 2
- System Structure & Program Execution 1에서 이어지는 강의

## 🍎 CPU내 프로그램 카운터의 역할
- CPU는 항상 메인 메모리의 Instruction(기계어)을 읽고 실행한다.
- 그럼 CPU는 Instruction이 어디있는지 알고 읽어올까?
- CPU안에 있는 레지스터 중에서 메모리 주소를 가리키고 있는 레지스터가 있다.
- 이것은 Program Counter Register이다.
- CPU는 Program Counter Register가 가리키고 있는 위치에서 Instruction을 하나 읽어와서 실행하고 메모리 주소를 가리키고 있던 Program Counter Register는 다음 주소를 가르키게 된다.
    - 교수님이 Instruction 하나가 보통 4 byte 정도 된다고 하셨다.
    - 즉, 다음 실행될 Instruction의 위치인 Program Counter가 4가 증가한다.
    - **프로그램에서 함수 호출이나 JUMP 같은 상황이 발생 할 수도 있으므로 메모리의 주소가 항상 4씩 늘어나는것은 아니다!**
- 즉, CPU는 메인 메모리에 있는 Instruction을 읽고 실행하는게 CPU의 역할이라고 했다.
- **조금 더 자세히는 CPU는 자신이 가지고 있는 레지스터 중 다음 실행 해야할 메모리 주소를 가리키고 있는 Program Counter를 통해 메모리에 접근하고, 그 안에 있는 Instruction을 실행하는 것이다.**

## 🍎 CPU가 하나의 Instruction을 실행하고 Interrupt Line을 체크한다고 했다.
- Interrupt Line을 체크한 후 프로세스는 어떻게 될까?
- CPU가 Interrupt Line에 와있는 Interrupt를 발견하면 자신의 소유권을 OS로 넘긴다.
- 그럼 OS는 Interrupt의 종류가 무엇인지 확인하고 인터럽트 당한 시점의 레지스터와 program counter를 save 한 후 CPU의 제어를 인터럽트 처리 루틴에 넘긴다.
    - 인터럽트 벡터
        - 인터럽트 번호와 처리 루틴을 쌍으로 가지는 딕셔너리라고 생각하면 됨
    - 인터럽트 처리 루틴
        - 실제로 인터럽트를 처리하는 커널 함수

## 🍎 Mode Bit에 따라 실행할 수 있는 Instruction의 범위가 다르다.
- Mode Bit이 0일때, 즉, OS가 CPU 소유권을 가지고 있을 때는 모든 Instruction 수행 가능
- Mode Bit이 1일때, 즉, 사용자 프로그램이 CPU 소유권을 가지고 있을 때는 제한된 Instruction 수행 가능

## 🍎 동기식 입출력과 비동기식 입출력
- 동기식 입출력 (synchronous I/O)
    - I/O 요청 후 입출력 작업이 완료된 후에야 제어가 사용자 프로그램에 넘어감
- 비동기식 입출력(asynchronous I/O)
    - I/O 요청 후 바로 다른 사용자 프로그램에 CPU 소유권 전달
