# OSI 7 Layers

## 🍎 OSI란?
- OSI는 컴퓨터 네트워크 프로토콜 디자인과 통신을 계층으로 나누어 설명한 것.
- 일반적으로 'OSI 7계층 모형' 이라고 한다.
- 이 모델은 프로토콜을 기능(계층) 별로 나눈것
- '프로토콜 스택' 또는 '스택'은 프로토콜 시스템이 구현된 시스템을 가리키는데, 프로토콜 스택은 하드웨어나 소프트웨어 또는 둘의 혼합으로 구현될 수 있다.
- **일반적으로 하위계층들은 하드웨어로, 상위 계층들은 소프트웨어로 구현**된다.

![](https://i.imgur.com/ioUUv37.jpg)

- 네트워크 통신 시 송신자와 수신자가 지켜야 할 각 단계의 규칙을 뜻하며, 위로 갈수록 Application(실제 프로그램 or 사용자)와 가까워 지며 아래로 갈수록 Physical(물리적인 기계)에 가까워 진다.
- 사용자가 Data를 보내면 아래로 내려 갈수록 **Protocol Header가 추가**되어 계층 정보(TCP,IP,MAC)을 덧 붙인다.
- **계층을 나눈 이유는 장애 발생시 어느 구간에서 발생했는지 보다 명확하게 알기 위함이다.**

## 🍎 필수로 이해해야하는 단계

- **Application Layer(7)**
    - 사용자가 UI로 접하는 응용 프로그램과 관련된 계층으로 HTTP,FTP,DHCP,SMTP,DNS 등이 있다. 여기에 속한 프로토콜들은 사용자와 직접 접하게 된다.
    - (Data + HTTP Header)
- **Transport Layer(4)**
    - 송신자와 수신자의 논리적 연결을 담당, 신뢰성 있는 연결을 유지할 수 있도록 도와준다.
    - End point(사용자)간 연결을 생성하고 데이터를 얼마나 보냈는지, 얼마나 받았는지, 제대로 받았는지 확인한다.
    - TCP, UDP가 대표적.
    - (Data + HTTP Header + TCP Header)
- **Network Layer(3)**
    - IP(Internet Protocol)이 활용되는 부분, 한 EndPoint가 다른 EndPoint로 가고자 할 때, 경로와 목적지를 찾아준다. 이를 Routing이라 하며, 대역이 다른 IP들이 목적지를 향해 제대로 찾아갈 수 있도록 돕는 역할을 한다.
    - (Data + HTTP Header + TCP Header + IP Header)

- **DataLink Layer(2)**
    - 같은 네트워크 대역을 사용하는 단말들에 대해 신뢰성 있는 전송을 보장. 즉, 'MAC address'를 활용하여 같은 구간 내의 Endpoint 혹은 Switching 장비에 전달하며, 1 Layer에 해당하는 물리 계층에 생길 수 있는 오류를 찾아낸다.
    - (Data + HTTP Header + TCP Header + IP Header + Ethernetframe Header)

- **Physical Layer(1)**
    - 'Cable'로 대표되는 물리적인 계층
    - 전기적 신호가 전송되는 구간

## 🍎 Citation
[네트워크 엔지니어 환영의 기술블로그](https://aws-hyoh.tistory.com/entry/OSI-7-Layer-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)
