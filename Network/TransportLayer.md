## 전송층
전송층은 네트워크층과 응용층 사이에 있다. 응용층에 서비스를 제공하는 층이다.

### 전송층 서비스

#### process to process communication
전송층 프로토콜의 첫번째 의무. 프로세스란 전송층의 서비스를 사용하는 응용층 객체(동작하는 프로그램)이다.<br>
전송층의 프로세스 대 프로세스 통신과 네트워크층의 호스트 대 호스트 통신의 차이를 간략히 짚자면<br>
호스트 대 호스트 통신은 단순히 목적지 컴퓨터에 메시지를 전달하는 것일 뿐이다. 예를 들어 카톡 메시지는 카톡 프로그램이 받아야하는데 이렇게 정확한 프로세스에게 메시지를 전달하는 게 전송층이다.<br>

#### 주소지정 : 포트번호
![image](https://user-images.githubusercontent.com/38284141/51530156-afc24300-1e7d-11e9-8c4c-3b0ce93c8df6.png)
클라이언트와 서버는 이렇게 포트번호를 갖고 자기 자신을 정의해야 한다. 차이점은 클라이언트 포트번호는 임시라는 거고 서버는 영구적인 번호를 쓴다.<br>
IP번호는 호스트를 정의하고 포트번호는 이 호스트에 있는 프로세스중에 하나를 정의한다!!<br>
IP주소와 포트번호의 조합을 소켓주소(socket address) 라고 한다.
![image](https://user-images.githubusercontent.com/38284141/51530931-62df6c00-1e7f-11e9-9957-2a8e13788cb1.png)

#### 캡슐화와 역캡슐화
메시지가 목적지 전송층에 도착하면 전송층은 헤더를 제거하고 메시지를 응용층의 프로세스에게 전달한다. 
![image](https://user-images.githubusercontent.com/38284141/51532039-717b5280-1e82-11e9-8f9c-136631d787e7.png)

#### 다중화 역다중화
다중화(multiplexing)은 프로그램이 근원지 1개 이상으로부터 메시지를 받아들이는 것.<br>
역다중화(demultiplexing)은 프로그램이 근원지 1개 이상으로 메시지를 전달하는 것.<br>
![image](https://user-images.githubusercontent.com/38284141/51532205-e484c900-1e82-11e9-9451-c65c15698ebb.png)
그림에서 클라이언트측 전송층이 다중화기로 프로세스 여러 개에서 메시지들을 받아들이고,<br>
서버측 전송층은 여러 메시지들을 역다중화기로 근원지 1개 이상으로 전달한다. 이때 근원지가 1개라도 역다중화기를 거친다.<br>

#### 흐름제어 오류제어
![image](https://user-images.githubusercontent.com/38284141/51534371-f79a9780-1e88-11e9-9074-5ae8723f397f.png)
![image](https://user-images.githubusercontent.com/38284141/51534391-02edc300-1e89-11e9-97ba-78901bc80bc3.png)<br>
pushing은 정보가 생성될 때마다 전송측에서 정보를 전달하는 경우. 이 경우에는 흐르제어가 필요함.<br>
pulling은 수신측이 요구한 경우에만 정보를 전달하는 경우. 흐름제어 불필요<br>
![image](https://user-images.githubusercontent.com/38284141/51534587-7bed1a80-1e89-11e9-9042-4ff3636dd3af.png)
송신측 전송층은 송신측 응용층이 보기에 수신측이고, 수신측 전송층이 보기에 송신측이다.<br>
<br>
전송층은 오류제어 서비스를 제공한다. 이건 오직 수신 전송층과 송신 전송층만 관여한다.<br>
즉 응용층과 전송층 사이에서는 오류가 없다고 가정한다.<br>
![image](https://user-images.githubusercontent.com/38284141/51534885-5e6c8080-1e8a-11e9-9793-627a88afbdba.png)


