# TCP/IP

## 🍎 TCP와 IP의 정의
- **TCP** : 전송 조절 프로토콜
    - IP 위에서 동작하는 프로토콜로, 데이터의 전달을 보증하고 보낸 순서대로 받게 해준다.
- **IP** : 패킷 통신 방식의 인터넷 프로토콜
    - 패킷 전달 여부를 보증하지 않고 패킷을 보낸 순서와 받는 순서가 다를 수 있다.
- **"TCP/IP를 사용하겠다"는 의미는 무엇일까?**
    - IP 주소 체계를 따르고 IP Routing을 이용해 목적지에 도달하며 TCP의 특성을 활용해 송신자와 수신자의 논리적 연결을 생성하고 신뢰성을 유지할 수 있도록 하겠다는 의미.
    - 즉, 송신자가 수신자에게 IP 주소를 사용하여 데이터를 전달하고 그 데이터가 제대로 갔는지, 너무 빠르지는 않았는지, 제대로 받았다고 연락은 오는지에 대한 이야기를 하는 것.

## 🍎 TCP/IP 4계층이란?
- OSI 7계층은 네트워크 통신을 표준화한 모델이다.
    - 통신 시스템을 7단계로 나누어 오류 발생시 어느 계층에서 오류가 났는지 쉽게 확인할 수 있는 장점.
    - 하지만 실무적으로 이용하기는 복잡하기에 **OSI 7계층을 단순화한 TCP/IP 4계층을 사용**한다.
    - **5,6계층을 7계층에 묶고**, 전기적 신호를 보내는 **물리계층과** '같은 네트워크 대역을 사용하는 단말들에 대해 신뢰성 있는 전송을 보장하는' **데이터링크 계층을 네트워크 인터페이스 계층으로 묶는다**.
![](https://i.imgur.com/GIk89j4.png)
- 위에서 부터 응용계층, 전송계층, 인터넷계층, 네트워크 인터페이스 계층 순서.

## 🍎 Encapsulation / Decapsulation 순서
- Encapsulation 순서
    - HTTP(7) -> TCP(4) -> IP(3)
- Decapsulation 순서
    - IP(3) -> TCP(4) -> HTTP(7)
## 🍎 TCP의 역할
- Internet Protocol(IP)는 패킷들의 관계를 이해하지 못하고 그저 목적지를 제대로 찾아가는 것에 중점을 둔다.
- TCP는 통신하고자 하는 양쪽 단말(Endpoint)이 통신할 준비가 되었는지, 제대로 전송 되었는지, 데이터에 이상은 없는지, 수신자가 받았을때 빠진 부분은 없는지 점검한다.
- 이런 정보는 TCP Header에 담겨있으며 SYN,ACK,FIN과 같은 신뢰성 보장과 흐름제어, 혼잡 제어에 관여할 수 있는 요소들이 포함되어있다. 또한 IP Header와 TCP Header를 제외한 TCP가 싣을수 있는 데이터 크기를 '세그먼트'라고 한다.

## 🍎 TCP의 작동
- TCP를 사용하는 송신자와 수신자는 데이터를 전송 하기전에 서로 통신이 가능한지 묻는다.(신뢰성 있는 통신을 하기 위함) 
- TCP Header의 **'SYN'**, **'SYN/ACK'**, **'ACK'**, flag를 사용해 통신을 시도한다.
![](https://i.imgur.com/jpRzH6G.png)
- 순서
    - 송신자가 수신자에게 'SYN'을 보내 통신이 가능한지 확인
    - 수신자가 송신자로부터 'SYN'을 받고 답장으로 'SYN/ACK'를 송신자에게 보내 통신할 준비가 되었다고 알려준다.
    - 송신자가 수신자의 'SYN/ACK' 답장을 받고 'ACK'를 날려 전송을 시작함을 알린다.
- 주고 받을 준비가 되어있어야 데이터를 안전하게 보내기 때문에 TCP로 이루어지는 모든 통신은 반드시 3-way handshake를 통해 시작.

## 🍎 TCP의 특징
- 흐름제어
    - TCP의 Header내 'Window size'를 사용해 한번에 받고/보낼 수 있는 데이터의 양을 정한다. 
    - [Window size](https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/description-tcp-features)는 주고 받을 데이터의 양.
    - Window size는 3-way handshake때 수신자가 정한다. 자신의 상황에 따라 Window size 조절.
    - 지금까지 받은 데이터 양을 확인해 송신자에게 보내는데 이를 'Acknowledgment Number'라고 한다.
    - 수신자가 300번째 데이터를 받았으면 'Acknowledgment Number'에 1을 더해 301을 송신자에게 보낸다. => "300번까지 받았으니 301번부터 보내!" 라는 의미.
    - 주고 받을 데이터의 순서를 표기한 것은 'Sequence Number'이다.
    - 즉, Sequence번호대로 송신자가 보내면 수신자는 데이터를 받고 Acknowledgment Number + 1을 송신자에게 반환해 다음 데이터를 받는다.
- 혼잡 제어
    - 데이터를 주고받는 양쪽의 단말(Endpoint)도 중요하지만 데이터가 지나가는 네트워크망의 혼잡 또한 중요하다.
    - 송신자와 수신자가 데이터를 넉넉히 주고받을 준비가 되어있어도 네트워크가 혼잡하다면 제대로 데이터를 보낼 수 없다.
    - '[Slow Start](https://www.stackpath.com/edge-academy/what-is-tcp-slow-start/)'알고리즘을 이용해 송신자는 연결 초기에 데이터 송출량을 낮게 잡고 보내면서 수신자의 수신을 확인하여 데이터 송출량을 늘린다. 이 과정을 거치면서 가장 적합한 데이터 송출량을 찾아 혼잡한 환경을 제어한다.


## 🍎 Citation
- [TCP/IP 쉽게 이해하기](https://aws-hyoh.tistory.com/entry/TCPIP-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)
- [TCP/IP 4계층 사진 출처](https://www.guru99.com/tcp-ip-model.html)
