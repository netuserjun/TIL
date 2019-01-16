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
