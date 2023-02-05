# System Structure & Program Execution 1
- 이번 챕터에서 계속 사용될 이미지.
- 이번 챕터는 프로그램들이 하드웨어 위에서 어떻게 작동하는지 설명한다.
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
        - CPU가 메모리에서 사용자 프로그램 A를 실행하다 프로그램이 '디스크에서 어떤 정보를 읽어오는 작업'이 필요하면 CPU가 직접 실행하는것이 아니라 CPU를 소유하고 있던 사용자 프로그램이 CPU 소유권을 운영체제(OS)에 넘겨주고 OS는 Device Controller에게 임무를 맡긴다.
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
    - OS는 Keyboard Controller에게 일을 시키고 다른 사용자 프로그램에게 CPU 소유권을 넘긴다.
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
- **만약 Interrupt Line에 timer가 보낸 interrupt가 있다면, CPU는 하던일을 멈추고 CPU는 자신의 제어권은 운영체제(OS)에게 자동으로 넘겨준다.**
    - 운영체제(OS)가 사용자 프로그램에게 CPU 소유권을 줄 때는 자유롭게 주지만, 한번 사용자 프로그램에게 CPU 사용권한을 주면 뺏어오지 못하고 운영체제가 다시 CPU의 제어권을 가지는 경우는 아래의 경우 뿐이다.

## 🍎 운영체제(OS)가 CPU를 다시 소유하는 방법
- 경우 1.
    - Interrupt Line에 timer가 보낸 timer interrupt가 있다면 사용자 프로그램이 가지고 있던 CPU 소유권을 OS에 반납
- 경우 2.
    - 사용자 프로그램이 수행해야 할 Instruction을 다 수행(즉, 프로그램 종료)하고 나면 자동으로 OS에게 CPU 소유권 반납.
- 경우 3.
    - CPU가 사용자 프로그램에서 Instruction을 읽다가 입력을 받아야하거나, 처리 결과를 화면에 출력해야 하면, 즉, I.O 작업을 수행 해야하면 CPU 소유권을 가지고 있던 사용자 프로그램은 I.O처리를 운영체제에 맡기기 위해 자동으로 CPU 소유권을 운영체제에게 반납한다.
        - 즉, 사용자 프로그램은 직접적으로 I.O를 수행할 수 없다.
        - **오직 OS만 I.O를 수행할 수 있도록 막아두었다.**
        - CPU 소유권을 받은 OS는 해야할 임무를 I.O Device Controller에 시킨다.
        - 느린 I.O 작업을 기다릴수 없는 OS는 다른 사용자 프로그램에게 CPU를 소유권을 넘겨 다른 작업이 수행될 수 있도록 한다.

## 🍎 그럼 I.O 처리를 위해 운영체제에게 CPU 소유권을 넘겼던 사용자 프로그램은 언제 다시 CPU 소유권을 가질까?
- 예를들어 **사용자 프로그램 X에서 키보드 입력을 요구한다고 가정 해보자**.
- 사용자 프로그램 X에서 I.O에 대한 처리 때문에 CPU 소유권을 OS에 넘기고, OS는 Keyboard Controller에게 입력에 대한 작업을 부탁한다.
- 사용자가 입력을 하고, 입력된 값은 local buffer에 저장된다. 이후, Keyboard Controller는 입력이 끝났다는 시그널을 Interrupt line을 통해 CPU에 전달한다.
- 그럼 다른 사용자 프로그램의 Instruction을 읽고 수행하던 CPU는 Interrupt line에 시그널이 와 있는것을 체크하고 자신의 제어권을 운영체제에게 넘겨준다.
- CPU 소유권을 받은 운영체제는 "어떤 Interrupt가 들어왔지??" 살펴보고 Keyboard Interrupt라는것을 인식한 운영체제는 아까 Keyboard local buffer에 저장되어있던 값을 키보드 입력 요청이 있었던 사용자 프로그램 X의 메모리 공간에 카피해주고, 방금 CPU의 소유권을 가지고 있던 다른 사용자 프로그램에게 다시 작업하라고 CPU 소유권을 넘겨준다.

            














