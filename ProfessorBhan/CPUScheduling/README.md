# CPU Scheduling
- 중요 개념:
- **운영체제는 프로세스들을 분류할 때 보통 사용자와 상호작용하는 프로세스들은 중요하다고 판단하고, 백그라운드에서 돌아가는 프로세스(batch process)들은 상대적으로 덜 중요하다고 판단하여 분류하는 것이 일반적**.

## 🍎 프로그램 실행에 있어 CPU burst와 I/O burst
![](https://i.imgur.com/O376W6p.png)
- **CPU만을 연속적으로 사용하는 단계를 CPU burst라고 부르고, I/O만을 사용하는 단계를 I/O burst라고 부른다.**
- 단 프로그램의 종류에 따라서 위 이미지처럼 CPU burst와 I/O burst가 빈번하게 있는 프로그램이 있고, CPU burst만 진득하게 있다가 I/O burst가 마지막에 한번 나오는 그런 프로그램이 있을 수 있다.
- 예를 들면, 사용자에게 빈번하게 입력을 요구하는 프로그램은 CPU burst 사이사이에 I/O burst가 많을것이고, 처음에 입력만 주어지면 계산 하고 마지막에 출력만 하는 프로그램이라면 I/O burst는 맨 앞과 맨 뒤에만 존재할 것이다.
- 유전자 염기서열을 분석하는 프로그램 처럼 입력이 주어지면 한번에 CPU를 오래쓰는 프로그램을 "CPU Intensive"한 프로그램이라고 한다.

## 🍎 프로세스의 특성 분류
- 프로세스는 그 특성에 따라 다음 두 가지로 나눔
    - I/O-bound process
        - CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job
        - many short CPU bursts
    - CPU-bound process
        - 계산 위주의 job
        - few very long CPU bursts
- CPU가 I/O-bound 프로세스를 너무 빈번하게 하면 CPU를 사용하는 효율이 떨어지고, CPU-bound 프로세스만 사용하면 사용자가 프로그램을 사용하는데 답답함을 느낄수 있다.
- 그래서 이 둘을 조화롭게 스케쥴링 하기 위해서 스케쥴러가 필요하다

## 🍎 CPU Scheduler & Dispatcher
- CPU Scheduler
    - Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다.
- Dispatcher
    - CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘긴다.
    - 이 과정을 context switch(문맥 교환)이라고 한다.
- **CPU Scheduler는 "누구한테 CPU를 줄까?" 라는 결정이고 Dispatcher는 실제로 CPU에게 주는 것이다.**
- CPU 스케쥴링이 필요한 경우는 프로세스에게 다음과 같은 상태 변화가 있는 경우이다.
    - 1. I/O 요청하는 시스템 콜 -> (running -> blocked)
        - 사용자 프로그램이 CPU를 OS에 반납하니까 자진 반납
    - 2. 할당시간 만료로 timer interrupt -> (running -> ready)
        - "너 CPU 사용할 수 있는 시간이 너무 오래 되었어! 반납해!"
    - 3. I/O 완료 후 인터럽트 -> (block -> ready)
        - "OS: 지금 방금 I/O 끝난 프로세스 있어 잠깐 그 프로세스한테 CPU 줘야해! 반납해!"
        - "실행중이던 프로세스: 시무룩..."
    - 4. terminate
        - "해야할 일 다 수행했으니까 반납할게"
- 1, 4에서의 스케쥴링은 **nonpreemptive**(=강제로 빼앗지 않고 자진 반납) -> 비선점
- All other scheduling is **preemptive**(=강제로 빼앗음) -> 선점

## 🍎 Scheduling Criteria
- Performance Index (성능 척도)라고도 한다.
- 성능척도는 다음 5가지로 산출 할 수 있다.
- 제일 중요! -> 이것은 CPU가 매 burst를 처리할 때를 기준으로 한다.
- CPU utilization (이용률)
    - 전체 시간 중 CPU를 실제로 가동한 시간의 비율
- Throughput (처리량)
    - 주어진 시간내 몇개의 작업을 완료했는지
- Turnaround time (소요시간, 반환시간)
    - CPU가 하나의 burst에 들어와서 실행하고 나갈때까지의 시간
- Waiting time (대기 시간)
    - 프로세스가 온전히 Ready Queue에서 기다린 시간
- Response time (응답 시간)
    - 요청이 제출되고 처음 반응이 생길 때 까지의 시간.

## 🍎 위의 내용을 중국집으로 비유
- 내가 중국집을 운영한다. -> 주방장을 계속 이용해야한다.
- 전체 시간중에서 주방장을 이용하는 시간 -> 이용률
- 특정 시간에 중국집에서 손님들을 받아 밥을 먹이고 내보내는 양 -> 처리량
- 손님이 중국집에 들어와서 주문하고 음식을 먹고 계산을하고 나가는데 까지 걸린시간 -> 소요시간
- 손님이 밥을 먹은 시간을 제외하고의 시간 -> 대기 시간
- 손님이 주문을하고 첫번째 음식이 나올때까지의 시간 -> 응답 시간
    - 짜장면을 주문해도 단무지가 나오면 주문 시간 ~ 단무지가 나온 시간이 된다.
    - 성능 척도에서 중요한 요소이다.

## 🍎 스케쥴링 알고리즘
### 📖 FCFS (First-Come First-Served)
- **먼저 온 순서대로 처리하는 스케쥴링.**
    - 비 선점형 스케쥴링이다.
    - 하나가 끝날때까지는 CPU를 내놓지 않는다.
    - 어떤 프로세스가 앞에 있느냐가 중요하다.
    - Convoy Effect -> 긴 시간이 걸리는 프로세스 뒤에 짧은 시간이 걸리는 프로세스
        - Convoy라는 말은 2차 세계전쟁에서 "앞에 물자가 있어 진군을 못하는 상황"
### 📖 SJF (Shortest-Job-First)
- **CPU를 짧게 쓰는(CPU Burst time이 짧은) 프로세스에게 CPU를 먼저 주는 스케쥴링**
- **가장 적은 평균 대기 시간 보장!**
- SJF 스케쥴링 알고리즘은 두가지로 나뉜다.
    - Nonpreemptive - 비선점
        - 일단 CPU를 잡으면 이번 CPU burst가 완료될 때까지 실행시간이 더 짧은 CPU burst가 와도 내어주지 않는다.
    - Preemtive - 선점
        - 현재 수행중인 프로세스의 남은 burst time보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗김
        - 이 방법을 Shortest-Remaining-Time-First(SRTF)라고도 부른다.
- SJF 스케쥴링 알고리즘은 극단적으로 시간이 짧은 job을 선호한다
### 📖 SJF의 문제점:
- 100ms 시간이 걸리는 작업이 있고, 계속 5ms의 작업이 들어온다면 100ms 작업은 영영 실행되지 않을것이다.
    - 그럼 다음에 올 프로세스가 얼마나 걸릴지 어떻게 알 수 있나?
        - 오직 추정(estimate)만이 가능하다.
        - 과거의 CPU burst time을 이용해서 추정
### 📖 Priority Scheduling
- 우선순위가 높은 프로세스에게 CPU 할당
    - SFJ는 일종의 priority scheduling이다.
    - 우선순위가 높을수록 숫자는 작다. ex) 1위 > 3위
    - 우선순위 스케쥴링도 Nonpreemptive / Preemptive로 나뉜다.
        - Nonpreemptive - 현재 실행하고 있는 CPU burst보다 높은 우선순위의 CPU burst가 들어와도 현재를 마칠때까지 CPU를 빼앗기지 않는다.
        - Preemptive - 현재 CPU burst 실행중 우선순위가 더 높은것이 들어오면 CPU 소유권을 넘겨준다. 
    - **Priority Scheduling도 마찬가지로 우선 순위가 다른 burst에 비해 낮다면 절대 실행되지 않을수 있다. 그래서 나온 기법이 Aging 기법이다.**
- Aging: 아무리 우선순위가 낮은 프로세스라도 오래 기다리면 우선순위를 서서히 높여준다.

### 📖 Round Robin (RR)
- **각 프로세스는 동일한 크기의 할당 시간(time quantum)을 가진다.**
- **적당한 time quantum을 주는것이 중요하다! 안 그러면 아래의 Performance에 영향을 준다!**
    - **일반적으로 10 - 100 ms**
- 할당 시간이 지나면 프로세스는 선점(preempted)당하고 ready queue의 제일 뒤에 가서 다시 줄을 선다.
- n개의 프로세스가 ready queue에 있고 할당 시간이 q time인 경우 각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n을 얻는다.
- **어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.**
- **Performance**
    - **q time이 크다면 -> FIFO**
    - **q time이 작다면 -> context switch 오버헤드가 커진다.**
- 일반적으로 SJF 보다 turnaround time이 길지만 response time은 더 짧다.
    - turnaround time -> 실행 시간과 대기 시간을 모두 합한 시간으로 작업이 완료될 때까지 걸린 시간
    - response time -> 프로세스가 처음 실행되기까지 걸린 시간으로 반응 시간이라고도 한다.

### 📖 Multi Level Queue
![](https://i.imgur.com/4io3GGo.png)
- 맨 위가 우선순위가 가장 높다.
- 맨 아래가 우선순위가 가장 낮다.
- CPU를 큐의 우선순위에 따라 할당하는 스케쥴링. 다단계 큐라고 불리기도 한다.
- 다단계 큐 알고리즘은 각각의 프로세스의 중요도에 따라 큐로 나누고 각 큐에서 다른 알고리즘을 적용해 효율을 높일 수 있다는 장점이 있다.
- 반면 한번 해당 큐에 들어가면 다른 큐로 이동할 수 없다!
- 즉, 스케쥴링 오버헤드(프로세스의 위치를 바꿀때 드는 비용)이 낮은대신 유연적이지 못하다
    - 1번 큐는 시스템 관련 프로세스들이 들어간다. -> (보통 운영체제가 만들어 낸 프로세스가 우선순위가 제일 높다.) (RR)
    - 2,3번 큐는 사람과 인터랙트 하는 프로세스들이 들어간다. -> (구글 아이디 입력처럼 사용자와 인터랙트 하게 동작하는 프로세스) (RR)
    - 4번 큐는 CPU를 오랜동안 사용하는 프로세스들이 들어간다. -> 게임 다운로드 (FCFS)
    - 5번 큐는 나머지 잔챙이 (FCFS)
- 멀티 레벨 큐에 프로세스가 들어올 때, 사용자와 빈번하게 인터랙트 한다면 1 ~ 3번 큐로, 시스템,사용자 인터랙션과 별로 상관이 없다면 4 or 5번큐로 들어간다.
- **문제는 OS가 프로세스의 중요도를 추측 후 큐에 넣는데, 이것이 잘못되어도 변경할 수 없다는 점이다.**
- 즉, **Multilevel Queue를 사용하면 프로세스는 처음 태어날때 정해진 순위로 끝날때까지 살아야한다.**
- **프로세스는 처음 태어날때의 신분을 변경할 수 없다!**
    - 성골은 성골로 쭈욱
    - 노비는 노비로 쭈욱
- **OS가 잘못 처음에 잘못 추측하면 효율적이지 못하다. 우선순위를 올릴수도 내릴수도 있어 효율적인 Multilevel Feedback Queue.**
- Multilevel Feedback Queue는 좀 이따 알아보자!
![](https://i.imgur.com/AKV54JB.png)
- 설명
- 위의 이미지에서는 큐가 5개이지만 이것을 "사용자와 인터랙션 하는 큐들을 모아 놓은 그룹"과 "그렇지 않은 큐들을 모아 놓은 그룹"으로 나누어 생각할 수  있다.
- 즉, 5가지의 Ready queue를 크게 2가지로 볼 수 있다.
    - foreground(interactive) 1 ~ 3
    - background(batch - no human interaction) 4 ~ 5
- 각 큐는 독립적인 스케쥴링 알고리즘을 가진다.
    - foreground - RR <- 사용자와 인터랙션을 해야하기 떄문에 RR 스케쥴링 알고리즘 사용
    - background - FCFS <- 사용자와 인터랙션을 하는 프로세스들이 들어있는 큐라서 FCFS로도 충분!
- 큐에 대한 스케쥴링이 필요 (두 종류가 있다)
- 하나는 우선순위가 높은 큐가 비어있을때만 낮은 우선순위 큐의 프로세스를 실행하는 Fixed priority scheduling.
    - **Fixed priority scheduling**
        - **foreground를 다 처리하고 background 처리**
        - **만약 4번 큐의 프로세스를 실행하던 중, 3번 큐에 프로세스가 들어온다면 4번 큐의 배치 프로세스는 잠시 뒤로 물러나고 우선 순위가 더 높은 3번 큐의 프로세스가 실행된다.(preemptive 방식)**
        - **그래서 잘못하면 background 큐들에게 CPU가 할당 되지 않을 수도 있다.**
- 또 다른 하나는 사용자와 인터랙션을 하는 큐에게 더 많은 비율을 할당하는 Time slice scheduling.
    - **Time slice**
        - **각 큐에 CPU time을 적절한 비율로 할당**
        - **80% to foreground in RR, 20% to background in FCFS**
        - **이 방법은 우선순위가 낮은 큐도 어쨋든 CPU를 잡기 때문에 starvation이 없다.**

### 📖 Multi Level Feedback Queue
![](https://i.imgur.com/TTTJSn0.png)
- Multilevel-feedback-queue scheduler를 정의하는 파라미터들
    - Queue의 수
    - 각 큐에서는 어떤 scheduling algorithm을 사용하는 기준
    - Process를 상위 큐로 보내는 기준
    - Process를 하위 큐로 내쫓는 기준
    - 프로세스가 CPU 서비스를 받으려 할 때 들어갈 큐를 결정하는 기준
- 프로세스가 다른 큐로 이동 가능.
![](https://i.imgur.com/LWnr5oP.png)
- 위 이미지에서, quantum = 8이라고 써있는 큐는 우선순위가 가장 높은 큐, quantum = 16이라고 써있는 큐는 우선순위가 그 다음으로 높은 큐, FCFS는 우선순위가 가장 낮은 큐.
- Multi Level Feedback Queue는 **처음 실행되는 프로세스를 우선순위가 가장 높은 큐**에 넣는다.
    - 우선순위가 가장 높은 큐는 RR 방식으로 quantum 시간을 굉장히 짧게 준다 (이미지에서는 8ms)
    - 밑으로 갈 수록 RR의 quantum 시간을 더 길게주고, 제일 아래 FCFS 방식을 사용한다.
- 만약 CPU burst가 아주 짧은 프로세스가 들어와서 quantum 시간내에 수행을 완료했다면 큐에서 바로 빠져 나간다.
- quantum 시간내에 수행을 완료하지 못했다면 프로세스는 강등이 되어서 우선순위가 다음으로 높은 큐에 넣어진다.
- 각 큐에 주어진 quantum시간 내에 처리를 하지 못하면 계속 강등이 된다.
    - **이 방식은 CPU burst 시간이 짧은 프로세스들에게 우선순위를 많이 주는 스케쥴링**
- **이 스케쥴링 알고리즘이 어떤 방식으로 작동하는지 알아보자!**
    - 보통 사용자와 interactive하지 않은 background의 프로세스는 CPU burst가 매우 크다는 특징이 있다.
    - 만약 용량이 엄청 큰 게임을 다운로드 한다고 생각해보자. 적어도 30분이 걸리는..
    - 그럼 우리는 다운을 받고 다른작업을 한다.
    - 그럼 어떻게 MLFQ 알고리즘이 게임 다운 받는것을 순위가 가장 낮은 큐에 넣을까?
    - 사실 "게임 다운 프로세스"도 가장 처음에는 "우선 순위가 가장 높은 큐"에 넣어주었다.
    - 다만 해당 큐는 quantum time이 너무 작기 때문에 바로 "게임 다운 프로세스"를 다음 큐로 넘겼을뿐.
    - 또 다음큐로 가고.. 다음큐로 가고.. 해당 프로세스의 우선순위가 쭈욱 밀려서 나중엔 배치 프로세스들을 처리하는 FCFS 큐에 들어가는것이다.
- 복습: 사용자의 입출력을 필요로하는 프로세스들은 I/O burst와 CPU burst가 작다는 특징을 가진다.
- 즉, 우선순위가 높다는 뜻이다.

### 📖 Multi Level Feedback Queue의 문제점
- 역시나 이 스케쥴링 알고리즘도 앞의 처리가 끝나야 다음 처리가 실행이 되는데 starvation 현상의 가능성이 있다.
- 낮은 우선순위 큐에서 너무 오래 기다리는 프로세스들을 높은 우선순위큐에 가져다 놓는 Aging 기법으로 해결 할 수 있다.

### 📖 Real-Time Scheduling
- Hard real-time systems
    - 정해진 시간 시간안에 반드시 끝내도록 스케쥴링 해야함
- Soft real-time computing
    - 일반 프로세스에 비해 높은 우선순위를 갖도록 해야함

### 📖 Thread Scheduling
- 쓰레드: 하나의 프로세스 안에 1 or more개의 CPU 수행단위
- Local Scheduling
    - 운영체제가 Thread의 존재를 모름!
    - User level thread의 경우 사용자 수준의 thread library에 의해 어떤 thread를 스케쥴 할지 결정
    - 즉, 프로세스 자체가 "어느 스레드에 CPU를 줄까?" 결정하는 것
- Global Scheduling
    - 운영체제가 Thread의 존재를 알고 있다!
    - Kernel level thread의 경우 일반 프로세스와 마찬 가지로 커널의 단기 스케쥴러가 어떤 thread를 스케쥴 할지 결정
    - 즉, CPU가 직접 "프로세스의 어떤 스레드에 CPU를 줄지 결정한다"

## 🍎 스케쥴링 알고리즘 평가
- Queueing models
    - 확률 분포로 주어지는 arrival rate와 service rate등을 통해 각종 performance index 값을 계산
    - 프로세스의 도착률과 처리율을 계산해서 처리량을 계산한다 -> 복잡!
    - **굉장히 이론적인 방법!**
    - **예전에 많이 썼던 방법, 요즘엔 실제로 프로그램에서 테스트한다!**
- Implementation & Measurement (구현 & 측정)
    - 실제 시스템에 알고리즘을 구현하여 실제 작업에 대해서 성능을 측정 비교
    - **운영체제 레벨까지 내려가서 직접 CPU burst 체크 -> 너무 어려워!**
- Simulation (모의 실험)
    - 알고리즘을 모의 프로그램으로 작성 후 trace를 입력으로 하여 결과 비교
    - **현대에 가장 많이 사용되는 알고리즘 평가!** -> 내가 사용할 프로그램에 알고리즘을 적용하고 여러가지 입력을 통해 성능 측정
