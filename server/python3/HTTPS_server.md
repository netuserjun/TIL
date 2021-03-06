## 파이썬으로 HTTPS 서버 구축하기

파이썬 3으로 HTTP 서버를 구축하는 2가지 방법이 있다.

#### python 명령어와 실행 옵션 만으로 HTTP 서버 구축

터미널 프로그램(윈도우는 cmd, 리눅스/OSX는 terminal)에서 python 명령어는 다양한 옵션을 제공하는데 <br>
그 중 -m 옵션은 명령어 실행과 동시에 실행할 모듈의 위치를 인자로 받는다. <br>
파이썬 3에서는 내장 웹 서버는 http.server 모듈이 제공한다.<br>

#### python 스크립트로 HTTP 서버 구축

스크립트로 내장 웹 서버를 구축할 때는 내장 웹 서버가 웹 서버의 요청을 받았을 때 <br>
어떤 파일이 어떻게 응답해야 할지 알려주는 핸들러 클래스를 지정해야 하는데 파이썬은 SimpleHTTPRequestHandler가 기본적으로 제공된다.<br>
예를 들어 내장 웹 서버가 HLS 파일을 제공하는 내장 웹 서버라면 <br>
파이썬 내장 웹 서버는 SimpleHTTPRequestHandler 클래스 변수인 extension_map 변숫값을 수정해서 <br>
특정 확장자인 m3u8, ts 파일이 어떤 MIMEType인지 브라우저에게 알려줘야 한다.<br>

파이썬으로 HTTPS 서버를 구축하려면 ssl 모듈을 임포트해야 한다..<br>
ssl 모듈은 소켓 통신에 대한 암호화 레이어 계층을 제공하는 모듈로서 openssl을 사용한다. <br>
덕분에 openssl이 설치되어 있으면 대부분의 운영체제에서도 암호화 레이어 계층을 쉽게 구현할 수 있는 기능을 제공한다.<br>
