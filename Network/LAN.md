# LAN
유선 랜과 무선랜, 가상랜에 대해 아라보자

## 유선 LAN 이더넷
이더넷은 다른 유선랜 기술이 망할 때 혼자 버텨냈다. 높은 데이터 전송률을 가진 버전으로 업그레이드됐기 때문이다.<br>

업그레이드 트리는 표준이더넷, 고속이더넷, 기가비트이더넷, 10기가비트이더넷 순서다.<br>

### 표준이더넷
최초의 이더넷이다. 표준이더넷의 특징만 살펴보자<br>
#### 비연결성 서비스
각 프레임이 독립적이라는 말이다. 연결 수립과 연결 종료 단계가 없는 이더넷은 수신자 준비여부에 상관없이 보낼 게 있으면 보낸다.<br>
수신자가 많은 패킷을 감당할 수 없어서 폐기해도 발신자는 모른다. <br>
그리고 패킷에 길이 제한이 있다. 최대 길이를 제한해서 버퍼크기가 작아지면 당시의 비싼 메모리 사용을 줄일수있었다.<br>
또 최대 길이를 제한함으로써 한 지국이 오랜 시간 공유채널 독점을 할 수 없게 했다.<br>
#### 주소지정
이더넷 네트워크에 있는 각 지국(PC, 워크스테이션, 프린터 등등)은 각자 네트워크 인터페이스 카드(NIC)를 가진다.<br>
이 NIC는 지국 내부에서 6바이트짜리 링크층 주소를 지국에게 제공한다.<br>
이 주소가 이더넷 MAC주소이다. 흔히 보는 형식인 16진법 표기로 4A:30:10:21:10:1A 이렇게 나타낸다.<br>
#### 접근방법
CSMA/CD 를 사용한다. 
<br><br> 

표준 이더넷은 기존네트워크 구성을 기존형태 > 브리지형 > 교환형 > 전이중 교환형 으로 점차 발전시켰다.<br>
전이중 교환형에서는 모든 링크가 점대점 전용링크라서 충돌이 없으므로 csma/cd가 필요없어졌다. <br>
또한 속도를 100Mbps까지 끌어내어 고속이더넷이라 불리게 된다.<br>

### 고속이더넷
표준이더넷에서 쓰던 프레임 형식, 최소프레임 길이, 최대 프레임 길이는 그대로 쓰인다.<br>
표준이더넷과의 호환성을 고려하면서 설계됐고, 몇 가지 기능이 추가됐다.
#### 자동협상
2개의 장비들이 데이터율, 동작모드를 협상할 수 있게 해주는 기능이다. 즉 100Mbps 장비와 10Mbps 장비가 통신할 수 있게 해준다.<br>
#### 물리층 변경
고속이더넷을 위해 물리층 설계가 약간 바뀐다. <br>
STP 2개 혹은 광섬유 케이블2개를 사용하는 방식과 URP 4개를 사용하는 방식이다. 각 방식별로 인코딩 방법이 다르다.<br>

### 기가비트 이더넷
기가비트이더넷의 목표는 데이터율을 1000Mbps로 올리되, 주소길이와 프레임형식, 최소최대 프레임 길이를 유지하는 것이다.<br>
하위 이더넷과의 호환을 고려한 것이기 때문에 고속이더넷의 자동협상 기능도 상속받는다.<br>
하지만 네트워크 길이는 최소 프레임 길이에 달려있는데, 최소 프레임 길이를 유지하고자 하면 최대 네트워크 길이가 고작 25m가 된다. <br>
사무실에서 25m로 랜을 만든다는 건...힘들다.<br>
#### 반송파 확장과 프레임버스팅
최소 프레임의 길이를 8배(512바이트) 증가시킨다. 그리고 더 커진 프레임에 작은 프레임 여러 개를 넣어 한 개처럼 보이게 해서 쓰레기값이 들어차는걸 방지한다.<br>
#### 물리층 변경
고속이더넷과 비슷한데 광섬유가 단파장/장파장 두가지로 나뉘어서 총 4가지 방법이 있다.

### 10기가비트 이더넷
개념만 나와있다.



## 무선 LAN
와이파이랑 블루투스 두 가지를 살펴본다.<br>

### 와이파이
ISM(Industrial, Scientific, Medical)밴드 대역 사용.<br>
여기서 ISM은 비면허 대역으로 규칙만 지키면 누구나 무료로 쓰는 대역이다. <br>
3개 대역이 있는데 902~928, 2.400~4.835, 5.725~5.850 이다.<br>

와이파이는 버전이 너무너무 많다. 중요 버전 몇가지만 살펴보자.
#### IEEE 802.11 FHSS(Frequency-hopping spread spectrum, 주파수 도약 확산 스팩트럼)
2.4 ~ 4.8 대역 사용. 1Mhz 대역폭79개 + 가드밴드로 나눠서 사용. 도약순서는 의사난수생성기로 선택.<br>
부대역으로 나누는 이유는 간섭때문이고 이걸 도약하면서 사용하면 보안성도 증가한다.
#### IEEE 802.11 DSSS (Direct sequence spread spectrum, 직접 시퀀스 확산 스펙트럼)
2.4 ~ 4.8 대역 사용. 
#### IEEE 802.11b HR-DSSS (high-rate Direct sequence spread spectrum, 고속 직접 시퀀스 확산 스펙트럼)
2.4 ~ 4.8 대역 사용. DSSS의 변조방식에 CCK(conplementary code keying, 보완적인 코드 입력)방식을 추가했다.
#### IEEE 802.11a OFDM ( orthogonal frequency division multiplexing, 직교 주파수 분할 다중화)
5.7 ~ 5.8 대역 사용. 52개 부대역으로 나누고 이중에서 48개는 비트그룹 송신에 쓰고 나머지 4개는 제어 정보 송신에 쓴다. 


### 블루투스
기기들이 짧은 거리에 있을 때 서로를 연결하기 위해 쓰는 무선랜 기술이다.<br>
그래서 PAN(Personal Area Network)라고 하는 작은 거실 크기만한 구역을 네트워크로 정의한다.<br>
네트워크 유형으로는 피코넷과 스캐터넷이라고 하는 두 가지 종류가 있다.
#### 피코넷(piconet)
지국을 8개까지 가질 수 있고, 하나가 주국 나머지가 종국이다.<br>
하지만 동시에 연결되는게 8개인거고, 추가로 8개를 통신에 참여하지 않은 상태(parked state)로 동기화는 시킬 수 있다.<br>
#### 스캐터넷(scatternet)
피코넷을 조합한 네트워크다. A피코넷의 종국이 B 피코넷의 주국이 되는 경우다. 
#### FHSS
블루투스는 2.4Ghz 대역 ISM밴드를 사용하고 이 대역을 79개 채널로 나눈다.<br>
위에서 와이파이가 사용한 것과 같다. 초당 1600번 도약을 하므로 한 주파수 당 625마이크로초 동안만 머문다.

## 가상랜 (virtual LAN)
LAN이 3개가 있고 LAN A에서 LAN B로 지국이 이동하는 경우에, 물리적인 네트워크 변경은 귀찮고 시간이 걸리니까 소프트웨어적으로 논리적인 이동을 할 수 있다. 이 논리적으로 구성된 랜을 VLAN이라고 부른다.<br>
이 방식의 장점은 그룹화로 인한 보안제공, 가상 워크그룹 생성, 물리적 재구성 불필요가 있다.




