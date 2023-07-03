# 3-way handshake

## 🍎 먼저 TCP 통신의 단계를 알아보자
- TCP 통신은 아래와 같은 3단계의 과정을 거친다.
    - Connection Setup (TCP 연결 초기화) - 3 way handshaking
    - Data transfer (데이터 전송)
    - Connection termination (TCP 연결 종료) - 4 way handshaking

## 🍎 3-way handshake란?
- TCP/IP 프로토콜로 통신하기전 정확한 정보 전송을 위해 두 단말(Endpoint)사이 세션을 수립하는 과정
- 클라이언트가 서버에게 접속을 요청하는 SYN(싱크로나이즈) 패킷을 보내면, 서버는 ACK(acknowledge)를 포함하여 SYN+ACK 패킷을 클라이언트에게 발송한다. 클라이언트가 이것을 수신한 후, 다시 ACK를 서버에게 발송하면 연결이 이뤄지고 데이터를 주고 받을수 있게된다.
- 순서
    - 클라이언트가 서버에게 SYN 패킷 전송 (서버야, 나 허락해줘)
    - 서버가 클라이언트에게 ACK + SYN 패킷 전송 (OK!, 클라이언트야 너도 나 허락해줘)
    - 클라이언트가 서버에게 ACK 패킷 전송 (OK! 허락했어! 데이터 보내!)

## 🍎 4-way handshake란?
- 3-way handshake는 TCP 프로토콜이 통신하기 전 세션을 수립하는 과정이였다면,
- 4-way handshake는 TCP 프로토콜로 수립했던 세션을 종료하는 과정이다. 즉, Connection termination이다.
- TCP Connection termination은 양방향으로 2개의 연결이 독립적으로 닫히기 때문에 4-way 단계를 밟는다.
- 순서
    - 클라이언트의 프로세스가 FIN 패킷을 서버의 프로세스로 보냄 (데이터 다 받았어! 클라이언트쪽 세션 문 닫을게!)
    - 서버가 ACK 패킷을 클라이언트로 보냄 (끝났구나! 알겠어!) (이때, 클라이언트는 서버의 FIN 패킷을 기다리고 있다.)
    - 서버가 FIN 패킷을 클라이언트로 보냄 (나도 끝났어! 서버쪽 션 문 닫을게!)
    - 클라이언트가 서버에게 ACK 패킷을 보냄 (알겠어! 고생했어!)
