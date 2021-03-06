# 데이터링크층 개요

데이터링크층은 물리층과 네트워크층 사이에 있는 층이다. 
두 컴퓨터가 서로 통신을 한다고 가정했을때 데이터링크층은 논리적으로 연결되어있다. 여기서 '논리적'이라는 말은 실제로 물리적연결은 아니지만 연결된것처럼
작동하는 것이다.<br>
데이터링크층은 노드 대 노드 연결을 책임진다. 여기서 노드는 컴퓨터, 라우터처럼 데이터를 주고받는 시스템을 총칭하는 말이다.<br>
책임을 다하기 위해 전달하는 측의 노드에 있는 데이터링크 층은 프레임에서 네트워크층으로부터 전달받은 데이터그램을 캡슐화해야한다. 그러니까 네트워크층에서 내려준 데이터그램이라는 것을 프레임이라는 약속된 박스안에 포장해서 넣는다.<br>
전달받은 노드, 라우터에서는 해당 프레임으로부터 데이터그램을 역캡슐화한다.<br>
여기서 라우터가 프레임을 까보는 이유는 링크(컴퓨터와 라우터 사이 통로)의 종류가 다를 수 있기 때문에, 라우터를 지나 이동할 링크가 다른 프레임형식을 사용한다면 알맞게 다시 포장해야한다.<br>
근데 프로토콜이 같더라도(같은 프레임형식) 링크 계층 주소(MAC주소, 링크주소, 물리주소 다 같은말이다. 헷깔리지 말자)가 달라지기 때문에 아무튼 까봐야한다.<br><br><br>

# 데이터링크층의 서비스

1. 프레임짜기<br>
   위에서 말한 데이터링크층이 제공하는 서비스다. 프레임은 헤더와 트레일러 모두를 갖고 있다.<br>
2. 흐름제어(Flow control)<br>
   한쪽 노드에서 열심히 포장을 해서 계속 보내면 받는 측에서 곤란하다. 수신측은 프레임들을 버퍼라는 창고에 넣는데 이 창고가 꽉 차면 프레임들을 폐기시키거나, 전송하는 측에 중지요청이나 천천히 보내달라는 요청을 한다.<br>
3. 오류제어(error control)<br>
   사실 프레임이 소포처럼 전달되는 건 아니고 데이터링크 층 밑에 있는 물리층이 이 소포를 전기적 신호로 바꿔서 보낸다. 그럼 받는 측에선 전기적 신호를 비트 형태로 변형해서 프레임으로 재조립한다. 이때 전기신호는 오류에 취약하기 때문에 프레임도 오류에 취약하게 된다. 프레임에서 오류가 검출되면 수신측에서 직접 수정가능한범위에 있으면 수정을 하기도 하고, 보낸측에 다시 보내달라고 요청할 수 있다. 오류제어는 내용이 방대하고 방법도 많아서 나중에 따로 다룬다. 데이터링크 층을 통과해서 상위 계층으로 올라간 프레임은 오류가 없단 것이다.<br>
4. 혼잡제어(congestion control)<br>
   이건 일반적으로 데이터링크층 보다는 네트워크층이나 전송측에서 더 다루는 개념이다. 대부분의 데이터링크층 프로토콜은 혼잡제어를 쓰지 않으니까 뒤로 미룬다.<br>
<br>


# 링크 계층 주소 지정

위에서도 말했다시피 링크계층주소, 링크레이어주소, MAC주소, 링크주소, 물리주소 모두 동일한 말이다. 공부하는 책이 이걸 혼용해서 쓰니까 다 알아둬야한다. ㅂㄷㅂㄷ<br>
통신에서 사용하는 주소에는 IP주소와 링크계층주소가 있다. 그런데 A,B가 서로 통신할 경우 상대방의 IP주소만 가지고 통신을 할 수는 없다.<br>
왜냐하면 서로 주고 받는 데이터그램들이 각각 경로가 다르기 때문이다. 즉 IP는 최종 목적지를 의미하는 논리적 주소일 뿐이고 링크계층주소가 실질적으로 데이터가 이동하기 위한 물리적 경로를 지정하는 주소이기 때문이다.<br>
데이터그램 속에 있는 IP주소는 절대 바뀌지 않는다. 만약 목적지 IP가 바뀌면 목적지로 제대로 못 찾아간다. 반대로 발신지 IP가 바뀌면 오류처리 등의 메세지를 보낼 수 없다.<br>
데이터그램이 프레임에 포장될때 프레임헤더에 두 데이터링크 주소가 추가된다. 그리고 이 두 주소는 링크가 바뀔때마다 변경된다.<br>
윗 문장이 좀 난해한데 내가 이해한바로는 프레임이라는 박스안에 담긴 데이터그램에는 발신지,목적지 IP주소가 있고 <br>
박스 겉면(헤더)에 발신지,목적지 데이터링크주소가 적힌다. 이걸 보고 목적지 데이터링크로 가서 박스를 뜯어보고 목적지 IP에 맞게 또 데이터링크 주소 두개를 겉면에 적는다. 이때 목적지였던 데이터링크주소 발신지가 되는 건 아니고 포트가 달라지니까 동일한 라우터의 새로운 주소가 적힌다.<br>
밑에 그림보면 이해하기 쉽다. N은 IP주소, L은 링크계층주소다.<br>
![image](https://user-images.githubusercontent.com/38284141/50768579-40b4ee00-12c4-11e9-9b5c-6decfe5ff4fa.png)<br>
<br>

# 주소 변환 프로토콜

네트워크 층에 ARP(address resolution protocol,주소 변환 프로토콜)라는게 있다. 이건 IP주소를 지정된 링크계층주소에 매핑시키고 데이터링크층에게 알려주는 프로토콜이라서 여기에 다룬다.<br>
호스트나 라우터는 네트워크에서 다른 노드의 링크계층주소를 찾기 위해 ARP요청 패킷을 브로드캐스팅한다.(즉, 해당 네트워크 내의 모든 호스트에게 보낸다. 라우터가 중간에서 다른 네트워크로 브로드캐스팅 패킷이 못가게 막는다.) 이 패킷에는 전송자의 링크계층주소,IP주소와 수신자의 IP주소를 담고있다. 이때 수신자 IP주소를 알 수 있는건 DNS덕분인데 ARP자체가 IP를 알아야 써먹을 수 있는 방법이다. 아무튼 브로드캐스팅된 패킷을 모두가 받았을 때 찾는 대상이 자기가 아니면 폐기하고, 자기가 맞다면 응답 패킷을 보낸다. 이때 응답 패킷은 브로드캐스트가 아닌 유니캐스트로 보내진다.<br><br>

만약에 호스트 A가 B를 찾기 위해 브로드캐스팅했는데 B가 해당 네트워크에 없다면 라우터가 "B는 여기 없어. 이거 내 링크계층주소니까 여기로 주면 내가 보내줄게"한다. 호스트는 그 라우터의 링크계층주소를 Default Gateway로 지정한다. 그리고 B로 보내고 싶은 패킷들을 Default Gateway로 모두 보낸다. 즉 B의 링크계층주소를 알 필요없이 라우터에게 떠넘기는 것이다. 착한 A의 라우터는 그 정보들을 받아 B의 IP주소를 가지고 연결된 라우터들에게 ARP로 브로드캐스팅한다. B가 살고있는 네트워크의 라우터가 응답해서 "B는 여기 네트워크에 살고있어. 이건 내 링크계층주소니까 나한테 주면 내가 B한테 보낼게."라고 한다. 그럼 이제 B의 라우터가 A의 라우터에게 정보들을 받아서 B의 IP주소를 이용해 ARP로 B의 링크계층주소를 찾는다. B는 해당 네트워크에 살고있으니까 ARP응답패킷을 라우터에게 보내고 드디어 B의 라우터는 정보들을 최종목적지로 보낼 수 있게 된다.<br>
다른 네트워크로 가는 과정은 이렇게 복잡하다. 라우터가 열일하는 걸 알 수 있다. 이 과정에서 기억해야 할 점은 ARP요청패킷이 각 구간별로 독립적으로 만들어지는 것이다. 참고로 ARP스푸핑 공격기법이 있는데 재밌는것같으니 나중에 아라보자<br><br>

ARP를 통해 알아낸 맥주소는(링크계층주소를 쓰기 귀찮으니 이하 맥주소로 통일한다.) 캐시메모리에 ARP 테이블로 저장해서 나중에 ARP요청패킷을 브로드캐스팅 할 필요없이 바로 해당 맥주소로 전송한다. 
<hr/>


