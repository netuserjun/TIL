# 네트워크층의 라우팅

네트워크층의 목적은 source에서 destination까지 데이터그램을 전송하는 것이다. 이 전송이 일대일 전송이라면 유니캐스트 라우팅(unicast routing),
일대다 전송이라면 멀티캐스트 라우팅(multicast routing)이라고 한다. 

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


