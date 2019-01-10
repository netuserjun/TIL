# 매체 접근 제어

데이터링크층이 가진 부계층 2개 중에 하나다. 노드나 지국들이 다중점 혹은 브로드캐스트 링크에 접근하는 것을 조율하는 것은 다중 접근 프로토콜이다.<br>
다중 접근 프로토콜은 여러 개의 노드들이 링크를 공유하는 것이기 때문에 많은 프로토콜이 존재한다.<br>
이 프로토콜들을 세 가지로 나눌 수 있다.<br>

#### 임의 접근 프로토콜 
종류로는 ALOHA, CSMA, CSMA/CD, CSMA/CA 등이 있는데 LAN과 WAN에서 사용한다.

#### 제어 접근 프로토콜
종류에는 예약, 폴링, 토큰통과 등이 있는데 일부만 LAN에서 사용한다.

#### 채널화 프로토콜
FDMA, TDMA, CDMA 등이 있고 전에 공부했던 LTE에서 봤듯이 휴대전화 무선통신에서 사용한다.<br><br>

## 임의 접근(random access)

임의 접근 혹은 경쟁(contention) 방식에서 모든 지국은 동등하다. 한 지국이 다른 지국을 제어할 수 없다.<br>
전송하고자 하는 각 지국은 매체의 상태를 확인하는 것을 포함한 미리 정해진 절차를 따라서 전송해야 한다.<br>
이 방식은 누가 언제 전송하는지 정해져 있지 않기 때문에 2개 이상의 지국이 동시에 전송을 시도하면 충돌이 나서 프레임이 손상될 수 있다. 그래서 이런 충돌을 해소하고자 다중 접근(MA,multiple access)라는 절차를 사용한다.<br>

### CSMA(carrier sense multiple access)
반송파 감지 다중 접근은 충돌을 최소화하기 위해 개발됐다. 이 방식에서 지국은 전송하기 전에 매체의 상태를 확인해야 한다.<br>
즉 누가 말을 하고 있는지 '말하기 전에 듣기'를 하는 것이다.<br>
하지만 어떤 지국이 말을 시작했지만 그 말이 모든 지국에게 들리기까지는 약간의 지연이 있다.<br>
그래서 그 지연시간 동안 다른 지국은 채널이 비어있다고 여기고 전송을 할 것이고 충돌이 발생한다.<br>
![image](https://user-images.githubusercontent.com/38284141/50904715-624cdb80-1464-11e9-8c66-713710fcde2a.png)


### CSMA/CD(carrier sense multiple access with collision detection)
CSMA는 충돌이 발생한 이후의 절차가 없다. 그래서 그 절차를 추가한 방식이다.<br>
밑에 그림에서 A가 보낸 프레임의 첫번째 비트를 C가 아직 못봤기때문에 A가 보냈음에도 C가 전송을 시작한다.<br>
이후 t3시간에 C가 충돌이 발생한 걸 인지하면 전송을 중단하고 해당 프레임을 버린다.<br>
A는 C가 보낸 첫번째 비트가 도달한 t4시간에 충돌을 인지하고 해당 프레임을 버린다.<br>
여기서 중요한 건 프레임의 크기다. (DLC의 정지/대기 프로토콜에서 지국은 프레임을 전송할 때 그 프레임의 사본을 만들어두고, 수신측으로부터 NAK을 받거나 / 응답 없이 타이머 시간이 다 된 경우 갖고 있던 프레임의 사본을 전송한다.)근데 MAC에서는, 아마도 이 프로토콜에서는 전체 프레임을 보내고나면 사본을 삭제한다고 한다.<br>
그러므로 프레임 전체를 다 보내기 전에 충돌이 감지되어야한다.<br>
그래야 프레임이 삭제되지 않고 다시 충돌없이 보낼 수 있기 때문이다.<br>
따라서 프레임을 전송하는 시간 = (프레임의 최대전파시간) * 2가 되어야한다. 신호의 왕복이니까 2를 곱하는거다.<br>

![image](https://user-images.githubusercontent.com/38284141/50904738-6ed13400-1464-11e9-8d50-ea618381bfff.png)

### CSMA/CA(carrier sense multiple access with collision avoidance)
얘는 무선 네트워크를 위해 정의된 방식이다. 충돌 회피를 위해 3가지 방식을 사용한다.<br>
프레임사이공간, 경쟁구간, 응답 이렇게 3가지다.<br>
#### 프레임 사이 공간
채널이 휴지 상태(노는상태)라고 감지해도 지국은 즉시 전송하지 않고 기다린다. 이 기다리는 시간을 '프레임 간 공간(interframe space)'라고 한다.<br>
이 시간을 둠으로써 멀리 있는 지국이 보낸 신호를 받을 여유가 생기는 것이다.<br>
그리고 이 시간을 중요한 지국에 짧게 지정함으로써 우선순위를 둘 수도 있다.<br>
만약 이 시간이 다 되도 채널이 휴지 상태면 지국은 밑에 설명할 경쟁 시간만큼 기다린다.<br>
#### 경쟁구간(contention window)
타임 슬롯으로 나뉜 일정 시간이다. 전송하고 싶은 지국은 임의의 수를 선택해서 그 수만큼의 슬롯동안 기다린다.<br>
처음에는 1개 슬롯으로 시작했다가 휴지상태가 아니라면 매번 두 배씩 슬롯을 증가시킨다.<br>
근데 가장 오래된 지국이 우선순위를 갖기 위해서 휴지상태에서 지나간 슬롯은 빼고 카운팅한다.<br>
위에 두 문장이 서로 충돌하는데 나도 뭔말인지 모르겟따. 그지같다. 책을 왤케 애매하게 번역한거야. 일단 저 두배씩 증가는 무시하자...<br>
#### 응답
위의 두 힘겨운 과정을 거친 후에 전송을 시작했어도 방심할 수 없다. 수신측에서 ack를 받아야 비로소 전송이 완료된 것이다.<br>

![image](https://user-images.githubusercontent.com/38284141/50909942-2c155900-1470-11e9-8df7-ee25342bcbe6.png)
<br>
#### NAV
![image](https://user-images.githubusercontent.com/38284141/50910836-f5d8d900-1471-11e9-89e1-e6fec561ae6c.png)
<br>
위 그림에서 NAV는 network allocation vector, 네트워크 할당 벡터라는 건데..<br>
어떤 지국A가 나 이제 보낼래요 하고 RTS(request to send)를 보내는데, 이 안에 얼마만큼의 시간동안 채널을 써야하는지 정보가 담겨있다.<br>
이걸 본 다른 지국들은 그 시간에 맞게 타이머를 설정한다. 타이머 이름이 NAV다.<br>
그리고 수신측이 보내세요 하고 CTS(Clear to send)를 보내면 A는 전송을 시작하고 다른 지국들은 타이머를 가동시킨다.<br>
NAV가 끝날 때까지 다른 지국들은 채널상태를 점검하지 않는다.<br>
근데 만약에 2개의 RTS 프레임이 충돌하면 CTS가 오지 않으니까 충돌이 났다고 여기고 정해진 대기시간 이후 다시 RTS를 보낸다.<br>


## 제어 접근(controlled access)
이 방법에서 지국들은 서로 누가 전송할 지 상의해서 정한다. 다른 지국에게 인정을 못 받으면 전송할 수 없다.
방식에는 예약, 폴링 토큰통과 등이 있다.
### 예약
지국은 데이터를 전송하려면 예약을 해야 한다. <br>
![image](https://user-images.githubusercontent.com/38284141/50912068-94663980-1474-11e9-8c0d-0f891bc8a881.png)
### 폴링
이 방식은 지국중 하나가 주국, 나머지가 종국이 되는 형태에서 동작한다.<br>
데이터들은 주국을 거쳐서 가게 된다. 주국이 링크를 제어하며 종국은 주국의 지시를 따라야한다. 언제 누가 쓰냐는 주국이 정한다.<br>
![image](https://user-images.githubusercontent.com/38284141/50912391-67665680-1475-11e9-8b4e-b292f79ad09a.png)
<br>
선택은 주국이 종국에게 받을 준비가 됐냐 물어보는것<br>
폴은 주국이 종국에게 보낼 거 있냐 물어보는것. NAK이면 다른 종국에게 물어본다. 종국은 보낼게 있으면 폴을 받자마자 데이터를 보낸다<br>
### 토큰 통과(token passing)
이 방식에서 네트워크의 지국들은 고리 형태로 구성된다. 그래서 각 지국은 선행자(predecessor)와 후행자(successor)를 가지게 된다.<br>
접근할 권리는 선행자 지국에서 현행 지국에게 넘겨진다. 현행 지국이 전송을 마치고 나면 후행자 지국에게 권리를 넘겨준다.<br>
이 권리가 담긴 것이 토큰이다. 토큰은 특별한 패킷으로 고리를 따라서 돌아다닌다.<br>
즉 토큰이 있는 지국만 데이터를 보내는 것이다.<br>
이 방식에서 필요한 기능은 한 기지국이 토큰을 갖고 있는 시간에 제한을 두는 것과 손상 감시다.<br>
토큰을 들고있는 지국이 맛탱이가 가면 아무도 말을 못하게 되기 때문이다.<br>
그리고 데이터의 종류에 우선순위를 두거나 지국에 우선순위를 둬서 토큰을 양보하게 하는 기능도 있어야한다.<br>

## 채널화(channelization)
#### FDMA(frequency division multiple access)
공유 채널의 가용 대역폭을 여러 개의 주파수 대역으로 쪼개서 각 지국에 분배.<br>
각 지국의 데이터링크층이 물리층에게 띠 대역 통과신호를 만들게 하고, 이 신호는 띠대역을 통과해서 공유채널로 전송된다.<br>
주파수 간섭을 줄이기 위해 각 대역 사이에는 약간의 가드밴드를 둔다.
#### TDMA(time division multiple access)
각 지국은 자기만 전송이 가능한 타임 슬롯을 할당받아 공유 채널에 전송한다.<br>
데이터링크층은 물리층에게 자신이 가진 타임 슬롯에 신호를 만들게 한다.<br>
기지국간 지연시간을 고려해 타임 슬롯사이에는 약간의 가드타임을 둔다.
#### CDMA(code division multiple access)
이건 여러명이 있는 한 방에서 두 사람만 같은 언어를 쓰는 방식이라고 이해하면 된다.<br>
<br>
채널화와 관련해서는 나중에 무선통신에서 자세하게 다룬다.


