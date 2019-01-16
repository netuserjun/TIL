# 네트워크층의 라우팅

네트워크층의 목적은 source에서 destination까지 데이터그램을 전송하는 것이다. 이 전송이 일대일 전송이라면 유니캐스트 라우팅(unicast routing),
일대다 전송이라면 멀티캐스트 라우팅(multicast routing)이라고 한다. 

## 인터넷 구조
라우팅에 대해 이해하려면 먼저 현재 인터넷 구조에 대해 지식이 있어야한다. 
![image](https://user-images.githubusercontent.com/38284141/51246312-857c0b80-19cd-11e9-8614-1f9ad7a640d1.png)
위 인터넷 구조 그림이 현재의 인터넷 구조다. 백본망은 글로벌 연결성을 제공하고, 이 백본들 사이를 연결해주는 피어링 포인트가 있고, 사용자 네트워크와 사용자 네트워크에게 서비스를 제공해주는 제공자 네트워크도 있다.<br>
ISP라고 하는 인터넷 서비스 제공자는 백본,provider network, customer network 들을 총칭하는 말이다.<br>
이 ISP가 네트워크와 라우터를 관리하는 방법은 AS(autonomous system), 즉 자율시스템이다. <br>
각 ISP를 하나의 AS로 취급하는 것을 계층적 라우팅이라고 한다.<br>
AS는 각각 라우팅 프로토콜을 가지는데 이걸 Intra-domain routing protocol이라고 하고, AS끼리를 연결하기 위한 글로벌 프로토콜을 Inter-domain routing protocol이라고 한다.

## 유니캐스트 라우팅
유니캐스트 라우팅에서 패킷은 라우팅테이블을 참조해서 홉 단위(라우터 단위)로 전달된다.<br>
유니캐스트 라우팅에는 3가지 대표적인 프로토콜이 있다. 거리벡터 알고리즘을 기반으로 하는 RIP, 링크상태 알고리즘을 기반으로 하는 OSPF, 경로벡터알고리즘을 
기반으로 하는 BGP다.

### RIP ( Routing Information protocol)
RIP는 위에서 말했듯이 거리벡터 라우팅을 기반으로 한다. 그럼 거리벡터 라우팅이 뭔지부터 알아보자.
#### 거리벡터(distance vector) 라우팅
먼저 거리벡터는 뭐지?<br>
거리벡터는 트리와 다르게 목적지까지의 최소비용만을 제공한다.
![image](https://user-images.githubusercontent.com/38284141/51239793-9f155700-19bd-11e9-9c47-7d0e3cdf4596.png)
그래도 서로 거리벡터를 교환하면서 밑에 나오는 벨만포드방정식으로 갱신을 한다면 궁극적으로 최소경로를 알 수 있게된다.<br>
<br>
위에서 말했듯 거리벡터 라우팅에서 라우터들은 자신의 모든 이웃들에게 자신이 가진 네트워크 정보를 불완전해도 계속 알려준다.<br>
그래서 불완전한 최소-비용트리가 만들어지면 벨만-포드 방정식(Bellman-Ford Equation)을 이용해 완전한 트리로 업그레이드된다.<br>
이 벨만포드 방정식은 거리벡터라우팅의 핵심이다. <br>
![image](https://user-images.githubusercontent.com/38284141/51239516-f49d3400-19bc-11e9-88cb-a04caabafe29.png)<br>
이 방정식을 반복적으로 사용하면 이전에 작성한 최소비용 트리로부터 새로운 최소비용 트리를 작성할 수 있다.
![image](https://user-images.githubusercontent.com/38284141/51241367-fec13180-19c0-11e9-9b7c-9c4bf16f0fd4.png)
<br>
#### RIP 알고리즘
RIP에서는 거리벡터 라우팅을 기반으로 하긴 해도 라우터가 목적지로 패킷을 보내기 위해서는 라우팅 테이블을 유지해야 한다.
아래 그림은 source에서 destination까지 홉 카운트를 보여준다. 그리고 그 라우터들에 대한 라우팅 테이블이다.<br>
![image](https://user-images.githubusercontent.com/38284141/51242165-a723c580-19c2-11e9-902c-93aa1958bb58.png)
![image](https://user-images.githubusercontent.com/38284141/51242341-14cff180-19c3-11e9-9c97-007a0d325a24.png)

아래 그림은 라우팅 테이블들을 갱신해주는 예제다.
![image](https://user-images.githubusercontent.com/38284141/51243809-db998080-19c6-11e9-8bcf-d59aeceb8e59.png)
#### 무한루프 문제
![image](https://user-images.githubusercontent.com/38284141/51249171-d2fc7680-19d5-11e9-88c1-249371101c7a.png)
A에서 X로 가는 경로에 문제가 생겨서 X 거리벡터를 16으로 지정했다. 근데 A의 이 변화내용을 B가 인지하기 전에 자기 거리벡터 2를 보내면 A는 B가 X에게 가는 길을 아는구나 싶어서 2+1로 3을 지정한다. 이 정보가 B에게 가면 B는 또 3+1로 4를 지정하고 이건 또 A한테 가서 5가된다. 이런식으로 16이 될때까지 반복하고 나서야 둘 다 아 못가는구나 깨닫는다. 이런 문제를 수평분할, 포이즌 리버스 등으로 완화할 수 있지만 완전한 해결책은 없다.

### OSPF(Open Shortest Path First)
얜 링크상태 알고리즘을 기반으로 한다.
#### 링크상태 알고리즘(Link-State algorithm)
![image](https://user-images.githubusercontent.com/38284141/51246816-a8f38600-19ce-11e9-8965-b9a7d00087f1.png)
링크상태 알고리즘은 그림처럼 각 라우터들이 LSP(Link State packet)를 교환한다. LSP는 전달해준 노드를 제외하고 다른 곳으로 다 보내서 갱신하도록 한다. 받은 LSP가 새로운 LSP가 아니면 폐기한다. 이렇게 완성된 LSDB(LS DataBase)를 라우터들이 공유한다.<br>

RIP와 다르게 OSPF는 비용을 홉 말고 링크의 처리량, 왕복시간, 신뢰성 등으로 가중치를 둘 수 있다. 만약 홉 수를 비용으로 쓰면 RIP와 똑같다.<br>
![image](https://user-images.githubusercontent.com/38284141/51249033-56699800-19d5-11e9-8a47-7fd02387664a.png)
![image](https://user-images.githubusercontent.com/38284141/51249042-5d90a600-19d5-11e9-977b-5f0f52d76440.png)
<br>
OSPF는 네트워크에 변화가 있을 때만 LSP를 주고 받으니까 RIP에 비해 트래픽이 적다. 하지만 위에 링크에 가중치를 두는 것처럼 기타 정보들을 더 포함해서 필요한 메모리는 더 크다.<br>
RIP보다 빠르게 네트워크 변화에 대처한다. 간단한 인증기능도 있다.<br>

### BGP(border gateway protocol)
유일한 인터도메인 라우팅 프로토콜. 문서 처음 인터넷 구조에서 말했듯 AS끼리의 연결에 사용하는 글로벌 프로토콜이다.<br>
#### External BGP vs Internal BGP
![image](https://user-images.githubusercontent.com/38284141/51250110-c88fac00-19d8-11e9-9a2c-7b3c28bff0d9.png)
간단히 eBGP는 서로 다른 as간의 연결을 위해 사용하고 iBGP는 동일한 as 내에서 사용한다.


## 멀티캐스트 라우팅
### DVMRP(Distance Vector Multicast Routing Protocol)
유니캐스트 라우팅에서 쓰던 RIP의 확장판이다. 멀티태스킹을 위해서 소스기반 멀티캐스트 트리를 만들어야 한다. 이 트리를 만들기 위해서는 세 단계를 거쳐야한다.
#### 역경로 포워딩(reverse pass forwarding)
출발지와 자신 사이의 최적 소스기반트리 생성
#### 역경로 브로드캐스팅(reverse pass broadcasting)
자신을 출발점으로 하고 노드는 인터넷의 모든 네트워크인 브로드캐스트 트리 생성
#### 역경로 멀티태스킹(reverse pass multicasting)
그룹의 회원이 아닌 노드를 트리에서 잘라냄으로써 멀티캐스트 트리 생성
![image](https://user-images.githubusercontent.com/38284141/51253628-355b7400-19e2-11e9-8954-45b6f82cd828.png)
그림에서 보듯 N은 RPF에서 패킷 두 개를 받는다. RPB에서는 부모노드를 지정해서 1개만 받고, RPM에서는 그룹이 아닌 노드를 잘라내서 패킷이 모두에게 전달되지 않게 한다.<br>

### MOSPF(Multicast Open Shortest Path First)
유니캐스트 라우팅에서 쓰는 OSPF의 확장판. 소스기반 트리 사용. 
![image](https://user-images.githubusercontent.com/38284141/51254350-3ee5db80-19e4-11e9-9f9e-5b32169bac73.png)
라우터는 최단 거리 경로 트리를 자기 자신이 아닌 S에서 생성한다. 즉 다른 라우터를 최상위 노드로 하고 자신이 서브 노드로 들어간다. 이게 가능한 이유는 LSDB가 있어서 인터넷 전체 토폴로지 정보를 알고 있기 때문이다.<br>
이렇게 트리를 생성하고 자기 자신을 찾은 뒤에 자신의 서브 노드들에게 브로드캐스트한다. 그리고 멤버가 아닌 노드들을 잘라내고 멀티캐스트 트리를 완성한다.

### PIM (Protocol Independent Multicast)
유니캐스트 라우팅프로토 콜종류와관계없이 사용가능한 멀티캐스트 프로토콜. DM, SM 두 가지 모드가 있다.
#### PIM-DM(PIM-Dense Mode)
밀집모드. 그룹의 엑티브 멤버가 많은 경우에 사용한다. 소스기반트리 사용. 위에 DVMRP에서 쓰던 RPF랑 RPM 사용하는데 DVMRP는 제거 메시지가 하위 라우터로부터 도착할 때까지 기다리고 멀티캐스트로 변경한다. 그래야 트래픽 낭비없이 패킷들을 보낼 수 있어서다. 그런데 여기 DM모드에서는 애초에 대부분의 라우터들이 그룹에 있다고 생각하니까 이렇게 제거 메시지를 기다리지 않고 바로바로 보낸다. 물론 제거 메시지가 도착하면 해당 노드를 쳐낸다.
![image](https://user-images.githubusercontent.com/38284141/51255224-25459380-19e6-11e9-859d-511116bccffa.png)
#### PIM-SM(PIM-Sparse Mode)
성긴모드. 번역이 이상한데 드문드문 이라는 뜻이다. 밀집의 반대.
![image](https://user-images.githubusercontent.com/38284141/51256997-0812c400-19ea-11e9-9849-5911441817ac.png)
RP는 Rendezvous point 랑데부 포인트, 즉 집결 지점이다. RP는 복잡한 알고리즘으로 정하는데 책에서는 알려주지 않는다. 일단 뇌용량을 넘어서니 나도 나중에 쓸 일이 생기면 알아볼거다. RP는 그룹마다 하나씩 있다는 것만 알아두고 넘어가자.<br>
아무튼 RP가 정해졌으니 여기에 참가를 원하는 네트워크는 join메시지를 보내서 멀티캐스트 트리를 만든다. <br>
만약에 네트워크 하나가 그룹에서 탈퇴하려면 prune메시지를 RP에게 보낸다.<br>
