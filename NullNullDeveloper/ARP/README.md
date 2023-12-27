# ARP
- Address Resolution Protocol
- 여기서 Address라는 것은 IP + MAC

## 🍎 ARP는 언제 필요한 프로토콜일까?
- ARP는 IP주소로 MAC주소를 알아내려 할 때 활용된다.
- 보통의 경우 PC를 부팅하면 Gateway의 MAC주소를 찾아내기 위해 ARP Request가 발생하며 이에 대응하는 Reply로 MAC주소를 알 수 있다.

## 🍎 그럼 Gateway의 MAC 주소는 왜 필요할까?
- 어떤 PC가 Naver 같은 호스트에 접속을 할 때, 목적지 MAC Address는 게이트웨이의 MAC 주소로 잡힌다.
