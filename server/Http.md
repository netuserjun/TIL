# Http
HyperText Transfer Protocol

http는 request와 response로 나뉜다.
굉장히 추상적인 개념인데
웹브라우저와 웹서버가 주고받는거다

웹브라우저는 사용자가 요청한 정보를 Request Headers를 만들어서 웹서버에게 대신 물어본다

Request message header format<br>
![http_request_header](https://user-images.githubusercontent.com/38284141/50723525-61454280-1122-11e9-8124-25f20da1e3d1.JPG)
<br>
출처 : https://codeflow.study/courses/74

첫 번째 행은 request line
그 다음 나오는 정보들은 request headers
이 둘을 합쳐서 request message header라고 한다.
