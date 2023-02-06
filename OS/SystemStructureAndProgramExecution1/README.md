# System Structure & Program Execution 1
- 이번 챕터에서 계속 사용될 이미지.
- 이번 챕터는 프로그램들이 하드웨어 위에서 어떻게 작동하는지 설명한다.
- 뒤로 갈수록 설명이 더욱 디테일 해진다.
![](https://i.imgur.com/622jVvZ.png)

## 🍎 컴퓨터의 구조
- 컴퓨터의 구조는 크게 세가지로 나뉜다.
    - CPU
    - Memory
    - I.O Devices(Disk도 I.O Device로 본다.)
- CPU
    - 중앙처리장치
    - 오직 메인 메모리하고만 일을 한다.
    - 더 정확히는 매 clock cycle마다 기계어로 된 Instruction을 메인 메모리에서 불러온다.
- Memory
    - 운영체제에서 메모리란 Main Memory(RAM)을 뜻한다.
    - CPU의 작업공간이다.
- I.O Devices
    - Disk를 포함한 입출력장치.
    - Disk는 왜 입출력장치일까?
    - Disk는 Memory에서 정보를 읽어들이는 Input Device의 역할도 하고, 또 처리결과를 하드디스크의 File System에 저장하면서 Output Device의 역할도 하기 때문에 I.O Device라고 할 수 있다.
    - 각각의 I.O Device는 각 Device를 전담하는 작은 CPU들이 붙어있다.
        - 이 작은 CPU는 위의 이미지에서 Device Controller(파란 작은 동그라미)이고, 디스크에 붙어있는 작은 CPU는 디스크 컨트롤러, 키보드에 붙어있다면 키보드 컨트롤러, 프린터에 있다면 프린트 컨트롤러, 모니터에 붙어있다면 모니터 컨트롤러라고 한다.
        - CPU가 메모리에서 사용자 프로그램 A를 실행하다 프로그램이 '디스크에서 어떤 정보를 읽어오는 작업'이 필요하면 CPU가 직접 실행하는것이 아니라 CPU를 소유하고 있던 사용자 프로그램이 CPU 소유권을 운영체제(OS)에 넘겨주고 Mode Bit을 0으로 바꿔준다(커널 모드).
        - OS는 CPU를 통해 Device Controller에게 임무를 맡긴다.
        - 즉, Disk에서 헤드가 어떻게 움직이고 어떤 데이터를 읽어들일건지 통제하는것은 각 Device 옆에 붙어 Device를 전담하는 작은 CPU(파란 작은 동그라미) Device Controller이다.
        - 쉽게 말해 각각의 I.O Device에 대한 통제 권한은 Device Controller가 가지고 있다.
    - Main CPU에서 작업공간인 Main Memory가 있듯이, 각 Device에 붙어있는 작은 CPU인 Device Controller에도 작업공간이 필요한데 이것을 local buffer라고 부른다.(위의 이미지에서 초록색 네모)

## 🍎 강의 중 교수님이 잠깐 이야기 하신것
- CPU와 I.O Device의 일 처리 속도는 약 100만배 차이가 난다. (하드디스크 기준)
- **CPU의 운명은 Main Memory에 있는 Instruction을 매 클럭 사이클마다 읽어들이는게 전부!**
- CPU 내, Main Memory보다 더 빠르면서 정보를 저장 할 수 있는 작은 공간이 있는데 이것을 Register라고 부른다.
- CPU 내, 지금 CPU에서 실행되는것이 운영체제인지, 사용자 프로그램인지 알려주는 Mode Bit이 있다.

## 🍎 인터럽트 라인(Interrupt Line)
- **CPU는 항상 Main Memory의 Instruction만을 읽어들이는 작업을 수행한다고 했다.**
    - 더 정확히는 Main Memory내 사용자 프로그램의 Instruction을 읽어들이는 작업을 수행한다.
    - Instruction을 읽어들이는 작업은 아래와 같다.
        - Instruction 하나가 실행되고 나면 기계어의 주소값이 증가하고 다음 클럭 사이클에서는 증가한 주소값에 있는 Instruction을 실행한다.
    - 이때 CPU가 입력을 요구하는 Instruction을 읽는다면 사용자 프로그램이 자신이 가지고 있던 CPU 소유권을 OS에게 넘겨준다.
    - Mode Bit을 0으로 바꿔주고, OS는 CPU를 통해 Keyboard Controller에게 일을 시키고 다른 사용자 프로그램에게 CPU 소유권을 넘긴다.
- 그럼 유저가 키보드를 통해 입력을 마치면, 마쳤다는 정보는 어떻게 받아올까?
    - **CPU가 I.O Device에서 보낸 정보를 받아오기 위해 존재하는것이 Interrupt Line이다.**

## 🍎 예시를 통해 인터럽트 라인이 어떻게 사용되는지와 Device Controller가 왜 필요한지 알아보자
- **CPU는 I.O Devices의 처리속도 대비 상당히 빠른 처리 속도를 가지고있다.**
- CPU를 사용자 프로그램 A가 가지고 있다고 하자.
- 프로그램이 CPU에서 Instruction을 수행하다가 프로그램 A가 유저로 부터 입력값을 원하는 Scanf와 같은 Instruction을 읽으면 키보드 입력을 해야한다.
    - 이때 발생하는 일은 아래와 같다.
        - 프로그램 A의 Instruction을 읽은 CPU는 유저의 입력을 받기위해 Device Controller인 Keyboard Controller에 일을 시킨다.
            - 왜? -> CPU는 사용자가 입력을 언제할지 모르니 마냥 기다릴 수 없다!
            - 그래서 Device Controller에게 다 되면 Interrupt를 걸어서 알려달라고 하고 바로 다음 CPU가 필요한 프로그램을 실행한다.
            - CPU가 해야하는 다른 작업은 많으니 CPU가 직접적으로 I.O Devices를 처리하지 않고 Device Controller에게 위임하는 이유이다.
        - 임무를 전달 받은 Keyboard Controller는 유저가 입력하면 그 정보를 local buffer에 저장하고 Keyboard Controller는 '임무가 종료되었다'라는 시그널을 인터럽트 라인을 통해 CPU에게 보낸다.

## 🍎 CPU가 프로그램의 Instruction을 읽는 중 무한루프를 만나면..?
- CPU는 시분할 처리 방식을 통해 여러 프로그램에 통제권을 주는데, 사용자 프로그램의 Instruction을 수행하는 중, 무한루프를 만나게 된다면 CPU는 하나의 프로그램에게 사용권이 묶인다.
- 그래서 컴퓨터 안에는 timer라는 하드웨어를 두고있다.

## 🍎 timer의 역할
- timer의 역할은 특정 프로그램이 CPU를 독점하는것을 막기위한 것이다.
- 우리가 컴퓨터를 켜면 처음엔 운영체제가 CPU를 가지고 있는다.
    - 왜? 이제 막 컴퓨터를 켰기 때문에 아무 프로그램이 실행되고 있지 않아서.
- 그러다가 사용자 프로그램들이 실행되면 프로그램들에게 CPU 사용권을 넘겨준다.
    - 여기서 CPU를 넘겨준다는 표현은 CPU를 사용할 권리를 준다는것이다.
- 하지만, 프로그램들에게 CPU를 넘겨줄 때, 그냥 넘겨주지 않는다.
- timer를 설정하고 넘겨준다. 이유는 위에서 이야기 했듯이 하나의 프로그램이 CPU를 독점하는것을 방지하기 때문이다.
    - 보통 하나의 프로그램이 CPU를 사용할수 있는 시간은 수 십 ~ 100 밀리세컨드다.
    - CPU를 넘겨받은 프로그램은 자신의 Instruction을 CPU에게 실행시킨다.
    - Instruction을 실행하다 timer에 설정된 시간이 되면 timer가 인터럽트 라인(Interrupt Line)을 통해 CPU에게 "하나의 프로그램에서 있어야 할 시간이 끝났어!"라는 timer interrupt 알림을 보낸다.
    - CPU는 timer interrupt를 받으면 자신의 소유권을 OS로 넘겨준다.

## 🍎 CPU가 일하는 방식
- CPU는 사용자 프로그램에 있는 Instruction을 수행한다고 알고있다.
- **진득하게 일을 하지 못하고 빨리 다른일을 하고 싶은 CPU는 하나의 Instruction을 수행하고 Interrupt Line에 무엇이 있는지 체크한다.**
- **만약 Interrupt Line에 timer가 보낸 interrupt가 있다면, CPU는 하던일을 잠시 멈추고 CPU는 자신을 소유할 수 있는 권한을 운영체제(OS)에게 자동으로 넘겨준다.**
    - 운영체제(OS)가 사용자 프로그램에게 CPU 소유권을 줄 때는 자유롭게 주지만, 한번 사용자 프로그램에게 CPU 사용권한을 주면 뺏어오지 못하고 운영체제가 다시 CPU의 제어권을 가지는 경우는 아래의 경우 뿐이다.

## 🍎 운영체제(OS)가 CPU를 다시 소유하는 방법
- 경우 1.
    - Interrupt Line에 timer가 보낸 timer interrupt가 있다면 사용자 프로그램이 가지고 있던 CPU 소유권을 OS에 반납
- 경우 2.
    - 사용자 프로그램이 수행해야 할 Instruction을 다 수행(즉, 프로그램 종료)하고 나면 자동으로 OS에게 CPU 소유권 반납.
- 경우 3.
    - CPU가 사용자 프로그램에서 Instruction을 읽다가 입력을 받아야하거나, 처리 결과를 화면에 출력해야 하면, 즉, I.O 작업을 수행 해야하면 CPU 소유권을 가지고 있던 사용자 프로그램은 I.O처리를 운영체제에 맡기기 위해 자동으로 CPU 소유권을 운영체제에게 반납한다.
        - 즉, 사용자 프로그램은 직접적으로 I.O를 수행할 수 없다.
        - **보안상의 이유로 오직 OS만 I.O 관련 업무를 수행할 수 있다.**
        - CPU 소유권을 받은 OS는 해야할 임무를 I.O Device Controller에 시킨다.
        - 느린 I.O 작업을 기다릴수 없는 OS는 다른 사용자 프로그램에게 CPU를 소유권을 넘겨 다른 작업이 수행될 수 있도록 한다.

## 🍎 그럼 I.O 처리를 위해 운영체제에게 CPU 소유권을 넘겼던 사용자 프로그램은 언제 다시 CPU 소유권을 가질까?
- 예를들어 **사용자 프로그램 X에서 키보드 입력을 요구한다고 가정 해보자**.
- 사용자 프로그램 X에서 I.O에 대한 처리 때문에 CPU 소유권을 OS에 넘기고, OS는 Keyboard Controller에게 입력에 대한 작업을 부탁한다.
- 사용자가 입력을 하고, 입력된 값이 local buffer에 저장된다. 이후, Keyboard Controller는 입력이 끝나 "자신의 local buffer에 무언가 있다"라는 Interrupt를 Interrupt line을 통해 CPU에 전달한다.
- 그럼 다른 사용자 프로그램 Y의 Instruction을 읽고 수행하던 CPU는 Interrupt line에 Interrupt가 와 있는것을 체크하고 자신의 제어권을 운영체제에게 넘겨준다.
- CPU 소유권을 받은 운영체제는 "어떤 Interrupt가 들어왔지??" 살펴보고 Keyboard Interrupt라는것을 인식한 운영체제는 아까 사용자 프로그램 X에서 입력을 요구했었지! 하면서 Keyboard local buffer에 저장 되어있던 값을 키보드 입력 요청이 있었던 사용자 프로그램 X의 메모리 공간에 카피해주고, 방금 CPU의 소유권을 가지고 작업하고 있던 다른 사용자 프로그램 Y 에게 다시 작업하라고 CPU 소유권을 넘겨준다.
- CPU 소유권을 받은 다른 사용자 프로그램 Y은 timer에 설정된 시간만큼 CPU를 통해 임무를 수행한다.

## 🍎 Mode Bit
- 사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호 장치이다.
- Mode Bit을 통해 하드웨어적으로 두 가지 모드의 수행
- 0 : 모니터 모드 또는 커널 모드라고 부른다.
    - 운영체제에서 CPU를 사용 중 이라는 이야기.
    - 무슨일이든 다 할 수 있게 정의되어있다.
        - 메모리 접근 뿐만 아니라 I.O Devices를 접근하는 Instruction도 실행할 수 있다.
- 1 : 사용자 모드
    - 사용자 프로그램에서 CPU를 사용 중 이라는 이야기.
    - 제한된 Instruction만 CPU에서 실행할 수 있다.
### 📖 만약 CPU가 사용자 프로그램에서 Instruction을 실행하다 I.O Devices에 접근하거나 OS의 메모리에 접근하려하면 어떻게 될까?
- CPU가 자신의 Mode Bit이 1인것을 확인하고 해당 작업을 하지 않도록 하드웨어적 구현이 되어있다.

### 📖 Mode Bit이 변하는 과정
- 사용자 프로그램에게 CPU를 넘기기 전에 Mode Bit을 1로 셋팅
- Interrupt나 Exception 발생시 하드웨어가 Mode Bit을 0으로 셋팅

## 🍎 Timer
- 정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트를 발생시킴
- 타이머는 매 클럭 틱 때마다 1씩 감소
- 타이머 값이 0이 되면 타이머 인터럽트 발생
- **CPU를 특정 프로그램이 독점하는 것으로부터 보호**
- 타이머는 시분할 처리 방식을 구현하기 위해 널리 이용됨
- 타이머는 현재 시간을 계산하기 위해서도 사용

## 🍎 Device Controller
- I.O Device Controller
    - 해당 I/O 장치유형을 관리하는 일종의 작은 CPU
    - 제어 정보를 위해 Controller 내 control register, status register를 가진다.
        - CPU가 Controller에 일을 요청할 때 Controller내 control register에게 업무를 전달한다.
    - local buffer를 가진다.(일종의 data register)
    - local buffer는 Controller의 작업 공간 및 저장 장소라고 생각하면 된다.
- **I/O는 실제 device와 local buffer 사이에서 일어난다.** 즉, Controller는 단순히 **제어만 한다**는 뜻!
- Device Controller는 I/O가 끝났을 경우 interrput로 CPU에 그 사실을 알림

### 📖 device driver와 device controller 차이
- device driver는 우리가 평소에 컴퓨터에 장치를 연결하면 해당 기기의 공식 웹사이트에서 다운로드 받는 소프트웨어이다
- device controller는 해당 장치를 통제하는 일종의 작은 CPU이다. 하드웨어!

### 📖 Main Memory 위에있는 memory controller는 무엇일까?
- 먼저 I.O Device에서 Device Controller와 local buffer를 보자!
- Device Controller는 작은 CPU이고 local buffer는 controller의 작업 공간이라고 했다.
- Main Memory를 local buffer의 역할로 생각하면 이를 통제하는 controller가 있어야 한다.
- 이것이 Main Memory를 통제하는 Memory Controller이다.

## 🍎 CPU는 사실 Main Memory와 local buffer 둘 다 접근이 가능하다.
- 단, Mode Bit이 0일때 가능한 이야기이다. (OS에서 CPU에 대한 소유권을 가지고 있을때)
- 오직 Main CPU만 Main Memory와 local buffer에 접근할 수 있고, 작은 CPU인 Device Controller는 자신이 가지고 있는 local buffer에만 접근이 가능하다.

## 🍎 CPU가 할일이 너무 많다! DMA Controller의 필요성!
- 사용자 프로그램에서 입력을 받는 경우를 생각해보자! 아래와 같은 프로세스로 진행된다.
- 사용자 프로그램에서 입력을 요구해서 OS에게 CPU 소유권을 넘겨준다.
- OS는 CPU에게 I/O에게 입력을 받아오라고 시킨다.
- CPU는 Mode Bit이 0인지 확인하고 Device Controller 내 control register에게 업무를 전달한다.
    - I/O는 실제 device와 local buffer 사이에서 일어난다!
- 그래서 local buffer가 가득차면 CPU에게 Interrupt를 보내는데, CPU는 Interrupt를 확인하고 해당 local buffer에 가서 정보를 가져와 사용자 프로그램의 메모리에 카피한다.
- 이런일이 너무 많이 반복되다보면 CPU가 너무 많은 Interrupt를 받아 효율성이 떨어진다.
- 이런 경우 때문에 DMA Controller라는것이 생겼다!

## 🍎 DMA Controller란 무엇일까?
- Direct Memory Access Controller.
- 원래는 Main Memory에 접근할 수 있는것은 CPU뿐이였는데, CPU의 역할을 분담하기 위해 DMA Controller를 만들고 Main Memory에도 접근 가능하도록 만들었다.
- 여기서 발생한 질문!
    - 그럼 CPU와 DMA Controller가 동시에 Main Memory 내 특정 사용자 프로그램의 메모리에 접근하면????
    - 이 경우를 중재 하는것이 Memory Controller의 역할이다.
- DMA Controller는 왜 필요할까?
    - I/O 장치가 너무 많은 Interrupt를 거니까 CPU가 방해를 너무 많이 받는다.
    - CPU가 I/O interrupt 체크하고 local buffer에서 데이터를 가지고 입력을 요구했던 프로그램의 메모리에 카피까지 해주는 역할을 하는것은 오버헤드가 크다.
    - DMA Controller의 역할은 CPU 대신 local buffer에서 값을 가져와 사용자 프로그램의 메모리에 값을 카피해주는 것까지 해준다.
    - 카피까지 끝나고 나면 CPU에게 Interrupt를 보내 "사용자 프로그램의 메모리에 카피까지 완료 되었습니다"라는 메세지를 보낸다.
    - 이렇게 CPU에게 가는 Interrupt의 횟수를 줄여 CPU를 더 효율적으로 사용하도록 하는것이 DMA Controller의 역할이다.


## 🍎 교수님이 CPU에 대한 막간 설명.
- CPU는 자신이 가지고 있는 레지스터 중에서 PC(program counter)라는 레지스터가 다음에 어느 Instruction을 실행해야 하는지 주소를 가지고 있기 때문에 이 레지스터에서 주소를 가져와 읽는다.
- Instruction를 읽다가 I/O 장치에 접근해야하는 상황이 생기면 device driver를 통해서 Device Controller에게 명령을 전달한다. 


## 🍎 입출력(I/O)의 수행
- 모든 입출력 명령은 특권 명령
    - 특권 명령은 OS만 수행 가능
- 사용자 프로그램은 어떻게 I/O를 하는가?
    - 시스템 콜(system call)
        - 현재 CPU의 소유권은 사용자 프로그램이 가지고 있기 때문에 Mode Bit은 1이다.  
        - 사용자 프로그램은 운영체제에게 I/O 요청할 필요가 있다.
        - **이것을 커널 함수를 부르는 행위 -> 시스템 콜이라고 한다.**
        - **커널 함수를 부르는 것은 사용자 프로그램 내에서 main 함수가 이 함수 저 함수 부르는것과 결이 다르다.**
        - 그럼 사용자 프로그램이 어떻게 운영체제에게 CPU 소유권을 넘길까?
        - Mode Bit이 1일때는 OS에 접근할 수 없다.
        - 그래서 사용자 프로그램은 직접 Interrupt line을 세팅하는 Instruction을 실행한다.
        - 즉, 사용자 프로그램이 I/O를 위한 Interrupt를 CPU에게 Interrupt line을 통해서 보내는것.
        - CPU는 하나의 Instrcution을 실행하고 Interrupt Line을 확인한다고 했다.
        - Interrupt Line에 I/O Interrupt가 들어온것을 확인하면 Mode Bit이 0으로 바뀌고, CPU의 소유권이 운영체제에게 넘어간다.
    - trap을 사용하여 인터럽트 벡터의 특정 위치로 이동
    - 제어권이 인터럽트 벡터가 가리키는 인터럽트 서비스 루틴으로 이동
    - 올바른 I/O 요청인지 확인 후 I/O 수행
    - I/O 완료 시 제어권을 시스템 콜 다음 명령으로 옮김

## 🍎 인터럽트
- 인터럽트 당한 시점의 레지스터와 Program counter를 save한 후 CPU의 제어를 인터럽트 처리 루틴에 넘긴다.
- 인터럽트는 크게 두가지로 나뉜다.
    - 하드웨어 인터럽트 -> Interrupt
        - timer + I/O
    - 소프트웨어 인터럽트 -> Trap
        - Exception : 프로그램이 오류를 범한 경우
        - System Call : 프로그램이 커널 함수를 호출하는 경우
- 인터럽트 관련 용어
    - 인터럽트 벡터
        - 해당 인터럽트의 처리 루틴 주소를 가지고 있음
        - 인터럽트를 어떻게 처리하는 방법들의 주소들을 가지고 있음
        - 실제 인터럽트를 처리하는 주소들을 가진 모음집이라고 생각하면됨
    - 인터럽트 처리 루틴
        - 인터럽트 핸들러 라고도 불리며
        - 해당 인터럽트를 처리하는 커널 함수이다.
        - 실제 인터럽트를 처리하는 코드라고 생각하면됨


## 🍎 I/O 처리를 위해서는 두가지 인터럽트를 처리해야한다.
- 1. 사용자 프로그램에서 OS에게 보낸 Software interrupt (trap) -> System Call
- 2. 그럼 OS가 CPU를 통해 Device Controller에게 일을 시킨다.
    - 시킨일이 다 끝나면 Hardware Interrupt가 걸린다.
