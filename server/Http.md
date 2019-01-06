# HTTP
HyperText Transfer Protocol

http는 request와 response로 나뉜다.
굉장히 추상적인 개념인데
웹브라우저와 웹서버가 주고받는거다

웹브라우저는 사용자가 요청한 정보를 Request Headers를 만들어서 웹서버에게 대신 물어본다

# Request message header format<br>
![http_request_header](https://user-images.githubusercontent.com/38284141/50723525-61454280-1122-11e9-8124-25f20da1e3d1.JPG)
<br>
출처 : https://codeflow.study/courses/74

첫 번째 행은 request line
그 다음 나오는 정보들은 request headers
이 둘을 합쳐서 request message header라고 한다.

서버로 전송할 정보(페이로드)가 있다면 바디부분에 적히는데
바디와 헤더는 사이에 한줄을 띄워서 구분한다.

# request line
GET은 데이터를 웹서버로부터 가져올때 쓰는 메소드(반대는 POST)
/doc/test.html 부분은 웹서버에게 요청하는 정보가 무엇인가
HTTP/1.1은 웹브라우저가 현재 사용하고 있는 혹은 사용할 수 있는 HTTP의 버전
<hr/>

# request header
Host는 반드시 있어야 하는 부분. 호스트는 컴퓨터를 식별하는 주소인데 요청하는 웹사이트의 주소를 적는거라고 보면 된다
하나의 웹서버가 여러개의 도메인을 서비스 할 수 있는데 
a라는 웹서버가 a.com b.com c.com이라는 도메인을 호스팅하고 있고 각각의 주소별로 다른 웹사이트라고 한다면
웹서버는 저 Host부분에 적힌 주소가 무엇인가에 따라서 다른 정보를 보내줄 수 있다. (가상호스트)
뒤에 :8080 이렇게 포트가 적힐 수 있는데, 
한대의 컴퓨터에는 여러개의 서버가 연결되어 있을 수 있는데 8080이라는 포트번호에 등록되어있는 웹서버를 의미한다.

User-Agent는 웹브라우저의 다른 표현. 요청하는 웹브라우저가 어떤 웹브라우저인지를 보여준다. CPU OS등등의 정보를 보내준다.
통계나 차단의 목적으로 사용한다.

Accept-Encoding
웹브라우저 웹서버가 서로 통신할때 응답하는 데이터의 양이 많을 때 압축을 사용할 수 있는데 
이때 웹브라우저가 어떤 압축방식을 지원하는가가 적힘.

IF-Modified_since
웹브라우저가 요청할 때마다 웹서버에게 계속 데이더를 다운로드 받는게 비효율적이기 때문에
마지막으로 웹브라우저가 다운받은 파일버전을 웹서버에게 알려줘서
웹서버는 응답할때 자기가 가진 파일과 비교해서 자기파일이 더 최신버전이라면 전송함. 버전이 같다면 전송안함.
<hr/>

# Response message header format<br>

![image](https://user-images.githubusercontent.com/38284141/50736727-4945ef80-1204-11e9-8d50-4e79764f38d2.png)




첫 번째 행은 request line
그 다음 나오는 정보들은 request headers
이 둘을 합쳐서 request message header라고 한다.
