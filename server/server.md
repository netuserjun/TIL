# TIL

# Http vs Socket

http는  웹브라우저에 정보를 표시하는 것과 같이 클라이언트의 요청이 있을 때 서버가 해당 페이지에 대한 자료를 전송하고 곧바로 연결을 끊는 방식
지금 이 깃헙 화면은 처음 로딩될때만 서버와 연결된거고 지금은 연결이 끊어진 상태
새로고침을 하면 서버랑 다시 연결됨. 
이렇게 하는 이유는 서버에 부하를 줄이기 위해. 시스템의 자원을 효과적으로 사용할 수 있음

Socket은 클라이언트가 서버와 접속이 되면 서버나 클라이언트에서 강제로 접속을 해제할 때까지는 계속해서 접속이 유지. 
따라서 서버의 능력이 무한대가 아닌 이상 동시에 접속할 수 있는 클라이언트의 수가 제한된다.
Socket 통신은 실시간으로 정보 교환이 필요하는 채팅이나 온라인 게임, 실시간 동영상 강좌 등에 사용. 
