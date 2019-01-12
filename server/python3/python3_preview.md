### Python3 서버 구현을 위해 공부시작

웹 브라우저의 요청 중에는 웹서버가 직접 처리할 수 없는 요청이 있다.<br>
파이썬은 서버쪽에서 동작하는 프로그래밍언어.
예를 들어 확장자가 .py인 경우 웹서버는 CGI(Common Gateway Interface) 기술을 통해 파이썬에게 넘겨준다.<br>

HTML파일 수십개를 만들 필요가 없고 구조적인 코드 수정이 간단하다.<br>
HTML을 프로그램적으로 생성해서 웹서버를 통해 응답하는 app을 구현해본다.<br>
환경은 비트나미를 이용해서 아파치웹서버를 사용하고, python3.7.2 버전, 에디터는 vs코드 <br>
이렇게 파이썬 사용방법을 먼저 익힌다.<br>

### CGI 설정방법
아파치 웹서버의 설정방법을 바꿔서 확장자가 py인 파일이 오면 파이썬으로 파일을 실행한 결과를 화면에 출력하게 하는 작업 <br>

난 testpy.py 라는 파이썬 파일을 만들었다. 내용은 아래와 같이 단순하다.<br>
![image](https://user-images.githubusercontent.com/38284141/51072104-a0cdda80-169e-11e9-8c19-44a27ace6f12.png)

a를 b로 나눈 결과인 2가 화면에 출력되게 만들건데, 지금 이대로는 그냥 파일내용이 출력된다. 그래서 설정을 만져준다.<br>
웹서버 설정을 바꾸는게 꽤 민감한 거라서 신중히 해야 한다.<br>

웹서버 설정파일은 비트나미 폴더내에 apache2 내에 있는 httpd.conf 파일이다.<br>
위에 말했듯이 잘못 건들면 문제가 생기니까 사진처럼 httpd_backup.conf라고 복사본을 만들어뒀다.<br>
![image](https://user-images.githubusercontent.com/38284141/51072020-97903e00-169d-11e9-90b3-96b78cac70aa.png)

<br>
httpd.conf 파일 내에 수정해야 하는 부분이 두 곳이 있다.<br>
먼저 mod_cgi.so 내용이 있는 문장이 주석처리(#)가 되어있지 않아야 한다.<br>
![image](https://user-images.githubusercontent.com/38284141/51072239-e68ba280-16a0-11e9-85db-2ced4578d354.png)

두 번째는 디렉토리부분 수정이다.<br>
Files ~ /Files 부분을 추가했다.<br>
.py 형식으로 들어오는 모든 파일에 대해 실행 결과를 보여주겠다는 내용이다.<br>
![image](https://user-images.githubusercontent.com/38284141/51072048-f1910380-169d-11e9-984e-58ad2ef5983b.png)

이렇게 하고 나면 설정이 끝난다. 적용을 위해 웹서버를 재시작하자.<br>
![image](https://user-images.githubusercontent.com/38284141/51072177-d0311700-169f-11e9-9a4a-ad17204da5b1.png)

그리고 실행을 해보면...<br>
![image](https://user-images.githubusercontent.com/38284141/51072123-e5f20c80-169e-11e9-9c45-c4553d8746ae.png)

500 에러, 즉 서버에서 문제가 생겼다는 말이다. 그럼 로그를 확인하자.<br>
아파치 웹서버는 로그를 다 저장해둔다. 그 내용은 logs라는 폴더 내에 있다.<br>  
이 중에서 에러 로그를 확인해보자.<br>
![image](https://user-images.githubusercontent.com/38284141/51072143-4123ff00-169f-11e9-8044-5631f099449d.png)

내 testpy.py가 실행불가능이라고 한다. 파일 첫줄에 #!로 무슨 언어를 쓰는지 적어달라고 한다.<br>
![image](https://user-images.githubusercontent.com/38284141/51072151-5d27a080-169f-11e9-8d2a-1642a1747967.png)

그래서 적어드렸습니다.<br>
![image](https://user-images.githubusercontent.com/38284141/51072165-a546c300-169f-11e9-9963-89aeab97f31a.png)

다시 실행해봤다.<br>
![image](https://user-images.githubusercontent.com/38284141/51072123-e5f20c80-169e-11e9-9c45-c4553d8746ae.png)

또 로그를 보자<br>
![image](https://user-images.githubusercontent.com/38284141/51072191-053d6980-16a0-11e9-83ad-bfbae3e2296e.png)

내 파일의 헤더가 형식이 안맞단다.<br>
![image](https://user-images.githubusercontent.com/38284141/51072198-3027bd80-16a0-11e9-93b9-2d109c43c511.png)

response header의 content-type과 같은 형식으로 보내주기 위해 파이썬 파일에 코드 한 줄을 추가하자<br>
![image](https://user-images.githubusercontent.com/38284141/51072216-6402e300-16a0-11e9-94d7-79fc2b05e355.png)
헤더랑 바디 사이엔 공백 라인으로 구분을 한다고 예전에 헤더 공부할 때 했었다. 그래서 '\n'으로 공백라인을 넣었다.<br>

이제 다시 웹페이지를 새로고침 해보면<br>
![image](https://user-images.githubusercontent.com/38284141/51072225-96144500-16a0-11e9-81e9-61e8eec91abf.png)
기다렸던 결과가 200과 함게 왔다.
