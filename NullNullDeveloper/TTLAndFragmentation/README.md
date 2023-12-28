# TTL과 단편화(Fragmentation)

## 🍎 라우터
- 라우터는 라우팅 테이블을 기반으로 라우팅한다.
- 인터넷은 라우터의 집합체라고 할 수 있다.
- 라우터와 L3 Switch
    - Router는 L3 Switch의 종류 중 하나라고 생각할 수 있다. (생각하기 나름)
- Internet는 Router와 DNS의 집합체

## 🍎 TTL
- Time To Live
    - 패킷이 최대 방문할 수 있는 라우터의 갯수.
    - 라우터를 하나 지나갈때마다 1씩 감소하고 0이되면 목적지를 찾지 못한것으로 간주되어 사라진다.
        - 패킷이 목적지를 찾지 못했는데에도 죽지않고 살아서 돌아다니면 네트워크를 마비시킨다.
- 하나의 라우터에서 다른 라우터로 넘어가는데 이 단위를 홉(Hop)이라고 한다.
    - 즉, 홉은 증가하고, TTL은 현재의 값에서 1을 줄인 상태로 보낸다.
- 전송했던 패킷이 목적지에 도달하지 못하면 처음 송신했던 PC에 알려주는 라우터도 있고, 보안상의 목적으로 알려주지 않는 라우터도 존재한다.
    - 패킷은 목적지까지 도달해야하는데 실패하는 경우가 있다.

## 🍎 단편화(Fragmentation)
- 일반적으로 많은 인터넷 트래픽은 표준 MTU 크기를 벗어나지 않아 단편화가 자주 발생하지 않는다.
    - 컴퓨터가 MTU 크기를 넘어가는 패킷을 보낼때 자르는 것을 의미한다.
    - MTU → 네트워크에서 한 번에 전송할 수 있는 데이터 패킷의 최대 크기
    - 결국 단편화된 패킷들을 조합하는것은 수신자이고, 이것은 다시 조립하는것은 효율적이지 못하다.
    - VPN을 사용하는 경우에는 단편화가 발생할 수도 있는데 이는 VPN 터널의 MTU 값은 1500보다 작은 경우가 많기 때문이다.