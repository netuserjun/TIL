내용 너무 길어서 분할함

## 맥 프레임 포멧

IEEE 802.15.4 문서는 맥 프레임의 네 가지 일반적인 구조를 정의했다. <br>
비콘프레임, 데이터 프레임, 확인응답 프레임, 맥 명령 프레임 <br>

### 일반적인 맥 프레임 형식

![image](https://user-images.githubusercontent.com/38284141/52693585-6d86b000-2faa-11e9-95a6-39191c02ce64.png)

위 그림의 a는 일반적인 맥 프레임이다. 그리고 이건 세 부분으로 이루어져 있다. <br>
#### MHR(MAC header), MAC payload, MFR(MAC footer) <br>
첫 번째 필드는 프레임 타입(비컨, 데이터, 수신확인, MAC 명령)을 정의하는 프레임 제어(b그림)이다. <br>
보안 가능 서브필드를 1로 설정하면, 이 프레임은 보안 보호가 되며, 보조 헤더는 MAC 프레임의 일부가 된다. 그렇지 않으면 보조 헤더의 크기가 0이다. <br>
프레임 미결 하위 필드는 간접 데이터 전송 방식의 일부로 사용되며, 1로 설정되면, 수신 디바이스를 위한 전송 장치에 보류 중인 데이터가 있음을 의미한다. <br>
확인응답 요청 하위 필드가 1로 설정된 경우 수신용 디바이스는 확인응답 프레임을 전송해야 한다. <br>
<br>
동일한 PAN 내에서 통신할 때, PAN 식별자는 근원지와 목적지 디바이스 모두에서 동일할 것이므로, 한 프레임에서 두 식별자를 모두 반복할 필요는 없다. 
PAN ID 압축 하위 필드는 PAN 식별자의 불필요한 반복을 방지하는 데 도움이 된다. <br>
PAN ID 압축 필드를 1로 설정하면, 오직 목적지 PAN 식별자만 프레임에 포함되며, 근원지 PAN 식별자는 목적지와 동일한 것으로 가정한다. <br><br>

목적지 및 근원지 주소 지정 모드 하위 필드는 주소 지정 모드(16비트 단축 주소 또는 64비트 확장 주소)를 결정한다.<br> 
MAC 프레임의 목적지 및 주소 필드의 길이는 주소 지정 모드에 따라 달라진다. 그 다음 서브필드는 프레임 버전이다. <br>
IEEE 802.15.4 표준은 시간이 경과함에 따라 갱신될 수 있으며, 프레임 버전 하위 필드는 프레임 구성에 사용되는 IEEE 802.15.4의 버전을 결정한다.<br>
<br>

MAC 프레임의 다음 필드인 시퀀스 번호는 비컨 시퀀스 번호(BSN) 또는 DSN(데이터 시퀀스 번호)을 포함할 수 있다. <br>
시퀀스 번호 매기기 기능은 다양한 시퀀스를 구분하는 데 도움이 된다.<br>
예를 들어 수신한 프레임 2개의 시퀀스 번호가 동일한 경우, 동일한 프레임이 재전송되었음을 의미한다. <br>
첫 번째 프레임이 성공적으로 검출되면 두 번째 프레임(같은 시퀀스 번호)은 무시할 수 있다.<br>
<br>

BSN과 DSN의 값은 MAC-PIB 속성(macBSN 및 macDSN )으로 저장된다. <br>
BSN은 비콘 프레임에서만 사용된다. DSN은 비콘 프레임을 제외한 모든 유형의 프레임에 사용된다. <br>
디바이스는 macDSN값(비콘프레임의 경우 macBSN)을 랜덤한 숫자로 초기화하고, 각각의 전송 후 1회씩 증가시킨다.<br>
<br>

보조 보안 헤더는 MAC 프레임의 선택 필드로서, MAC 프레임을 보호하기 위해 사용되는 보안 레벨 및 보안 키의 종류 등의 정보를 담고 있다. <br>
MAC 프레임의 마지막 필드는 수신자가 수신된 프레임에서 발생 가능한 오류를 확인하기 위해 사용하는 FCS(프레임 검사 시퀀스) 필드다.<br>
<br>

### 프레임 체크 시퀀스 FCS(Frame Check Sequence)
IEEE 802.15.4는 데이터 패킷에서 가능한 오류를 검출하기 위해 국제 전기통신 연합(ITU) 주기적 중복 검사(CRC)에 기초한 16비트 FCS를 사용한다.<br>

CRC의 기본 개념은 다음과 같다. <br>
송신 장치에서는, MHR과 MAC 페이로드의 모든 비트는 다항식의 계수로 취급된다. <br>
그런 다음 이 다항식은 수신기와 송신기 둘 다에 의해 알려진 또 다른 다항식으로 구분된다. <br>
이 분할의 나머지 부분은 FCS라고 불리며 MAC 푸터(MFR)로서 프레임 끝에 추가된다. <br>
수신 장치는 동일한 분할을 수행하며 동일한 나머지를 얻을 것으로 예상한다. <br>
수신 장치에 의해 계산되는 나머지가 MFR의 송신 장치에 의해 제공된 나머지 부분과 동일하지 않으면, 수신 장치는 프레임이 오류로 수신된다고 결론짓는다.
<br><br>

IEEE 802.15.4의 FCS는 다음 단계를 거치면서 생성된다.<br>
1. 비트의 MHR 및 MAC 페이로드 시퀀스를 나타내는 다항식 M(x)을 정의한다.<br>
2. M(x)을 x^16로 곱하여 M(x) x^16을 생성한다.<br>
3. 다항식 M(x) * x^16를 (x^16 + x^12 + x^5 + 1)로 나눈다. <br>
이 나눗셈의 나머지를 프레임 체크 시퀀스로써 MFR에 넣는다. <br>

### 맥 비콘 프레임 포멧
비콘 프레임(밑에 그림)은 MAC 일반 프레임 형식의 특별한 형식이다. <br>
Sequence number 필드는 macBSN(beacon sequence number)의 현재 값을 포함하고 있다. <br>
![image](https://user-images.githubusercontent.com/38284141/52696878-82b40c80-2fb3-11e9-850b-0a0ba858a9a4.png)
<br>
슈퍼프레임 사양 필드는 MAC 페이로드의 일부로서, 그 서브 필드는 위 그림에 나타나 있다. <br>
비콘 순서(BO) 서브 필드는 비콘 사이의 전송 간격을 결정한다. <br>
SO(Superframe Order)에서는 슈퍼프레임의 활성기간을 명시하고 있다.<br>
<br>

슈퍼프레임은 16개의 동일한 시간 슬롯으로 나뉘며, 최종 CAP 슬롯 하위 필드는 CAP의 마지막 시간 슬롯을 결정한다.<br>
CAP 후 남은 시간 간격은 GTS로 사용한다. <br>
BLE 서브필드가 1로 설정되어 있는 경우, 에너지 절약을 위해 일정 시간 후에 비콘 디바이스가 수신기를 끄게 된다는 것을 의미한다.<br>
<br>

이 비콘 프레임이 PAN 코디네이터에 의해 송신되는 경우, PAN 코디네이터 하위 필드는 1로 설정된다. <br>
이것은 PAN 코디네이터와 같은 네트워크의 다른 코디네이터로부터 수신한 프레임을 구별하는 데 도움이 된다. <br>
슈퍼프레임 사양 필드의 마지막 비트는 연결 허가다. <br>
연결 허가가 0으로 설정되어 있으면, 코디네이터가 현시점에서 연결 요청을 받아들이지 않는 것을 의미한다.<br>
<br>

비콘 프레임은 GTS를 만드는 데 사용된다. <br>
GTS 사양 하위 필드는 PAN 코디네이터가 현재 GTS 요청을 수락하는지 여부를 명확하게 한다. <br>
또한 GTS 목록 필드에 열거된 GTS의 수를 결정한다. <br>
GTS 방향 하위 필드는 GTS의 방향을 수신 전용 또는 송신 전용 모드로 설정하는 데 사용할 수 있다. <br>
GTS 리스트는 현재 유지되고 있는 모든 GTS 리스트(GTS 설명자)이다. <br>
각 GTS 설명자는 GTS, GTS 시작 슬롯 및 GTS 길이를 사용할 디바이스의 짧은 주소를 포함하고 있다.<br>
<br>

보류 중인 주소 필드에는 코디네이터에서 보류 중인 데이터가 있는 모든 장치의 주소가 포함되어 있다. <br>
각 장치는 이 필드의 주소를 확인하고, 일치하는 항목이 있으면 코디네이터에게 연락하여 데이터가 전송되도록 요청한다. <br>
비콘 페이로드는 선택 필드로서 다음 상위 계층(NWK)에 의해 제공된다. <br>
<br>

### 맥 데이터 프레임 형식
![image](https://user-images.githubusercontent.com/38284141/52697920-cf004c00-2fb5-11e9-917e-43d69ac78df1.png)
<br>
MAC 데이터 프레임은 그림a와 같다. 데이터 페이로드는 NWK 레이어에 의해 MSDU로 제공된다.<br>
MAC 데이터 프레임을 MPDU라고 부른다. <br>
시퀀스 번호 필드는 프레임이 생성되었을 때 현재 macDSN(데이터 시퀀스 번호)의 값과 같다.<br>

### 맥 확인응답 프레임 형식

그림 b에 나타낸 MAC 확인응답 프레임은 수신 장치에 의해 선택적으로 전송되어 패킷의 성공적인 수신이 확인된다. <br>
또한 이 프레임을 사용해 수신자가 해당 장치에 자신을 위한 보류 중인 데이터가 있음을 알 수 있다. <br>
이것은 프레임 제어 필드의 프레임 보류 하위 필드를 1로 설정함으로써 이루어진다. <br>
시퀀스 번호 필드는 현재 macDSN 의 값과 같다.<br>
<br>

### 맥 명령 프레임 형식
MAC 명령 프레임(그림 c)은 MAC 계층 명령을 수신 장치에 전달하는 데 사용된다. <br>
시퀀스 번호 필드는 확인응답되는 프레임의 현재 macDSN 값과 동일하다. <br>
MAC 명령 목록은 밑에 표에 수록되어 있다. 명령 프레임 식별자 필드는 표의 명령 중 어떤 명령이 실행될지 결정한다. <br>
명령 자체와 모든 관련 파라미터는 명령 페이로드에 배치된다. <br>
FFD는 모든 명령을 전송하고 수신할 수 있어야 한다. <br>
그러나 RFD는 표에 체크 마크로 표시된 명령을 수신하거나 전송할 수 있어야 한다. <br>
![image](https://user-images.githubusercontent.com/38284141/52708201-f0b8fd80-2fcc-11e9-9ef2-9f779777363c.png)
<br>

###  The Association Request and Response Command Formats 연결 요청과 응답 명령 형식
![image](https://user-images.githubusercontent.com/38284141/52708515-b2700e00-2fcd-11e9-8d63-6dead8862047.png)
<br>
연결 명령은 MAC 연결 서비스의 일부로 사용된다. <br>
연결 요청 및 응답의 형식은 그림 3.25와 같다. <br>
연결 요청의 기능 정보 필드는 네트워크 가입을 요청한 장치에 관한 다음과 같은 질문에 대답하는 데 도움이 된다.<br>

● 장치가 PAN 코디네이터 역할을 할 수 있는가? <br>
  기기가 PAN 코디네이터 역할을 할 수 없는 경우 대체용 PAN 코디네이터 필드를 0으로 설정한다. 이것은 FFD인가 RFD인가? <br>
  장치형 필드가 1로 설정되어 있는 경우 장치는 FFD이다. <br>
● 디바이스가 배터리 전원을 쓰는가 상전을 쓰는가? <br>
  배터리 구동 장치의 경우, 전원 리소스 필드는 0으로 설정되어 있다. <br>
● 기기가 항상 수신기를 ON 모드로 유지하거나, 기기가 한가한 모드에 있을 때 수신기를 끄고 절전 모드로 가는가? <br>
  한가한 모드 중 수신기가 꺼져 있으면, 한가한 필드가 0으로 설정되어 있을 때 수신기가 ON이다. <br>
  이 필드의 내용은 MAC 속성 macRxOnWhenIdle 에서 나오는데, 이는 한가한 모드 중에 수신기가 꺼져 있을 경우 FALSE로 설정된다.<br>
● 기기가 암호로 보호된 MAC 프레임을 수신·송신할 수 있는가? <br>
  가능한 경우 보안능력필드를 1로 설정한다. <br>
● 네트워크 가입 후 디바이스가 새로운 주소를 갖고자 하는가? <br>
  장치가 짧은 주소를 필요로 하는 경우, 할당 주소 필드는 1로 설정된다.<br>
<br>

코디네이터는 연결 응답 명령을 사용하여 장치의 연결요청을 수락하거나 거부한다. <br>
코디네이터가 수용량에 도달했을 경우, 연결응답은 용량 상태에 PAN을 표시한다. <br>
연결 요청이 받아들여지고 장치도 새로운 주소를 요청한 경우, 연결 응답의 단축 주소 필드에 단축 주소가 제공된다.<br>

### The Disassociation Notification Command Format  해제 알림 명령 형식
연결 해제란 연결된 기기가 코디네이터에게 기기가 네트워크를 떠날 의도가 있음을 통지하기 위해 사용하는 절차다.<br>

![image](https://user-images.githubusercontent.com/38284141/52710110-ddf4f780-2fd1-11e9-8a3b-c241f67cc9f4.png)
<br>
해제 프레임 형식은 위 그림에 나타나 있다. 장치는 자체 결정 또는 코디네이터의 요청에 기초하여 네트워크를 떠날 것이다.<br>
<br>

###  The Data Request Command Format 데이터 요청 명령 형식
![image](https://user-images.githubusercontent.com/38284141/52710233-31674580-2fd2-11e9-8e00-6e9e00cc1172.png)
<br>
데이터 요청 명령의 MAC 페이로드에는 명령 프레임 식별자만 포함되어 있다. <br>
이 명령은 간접 데이터 전송 방법의 일부로 코디네이터에게 데이터를 요청할 때 사용한다. <br>
기기가 비콘 프레임의 보류 주소 필드에서 주소를 찾으면 코디네이터에 보류 중인 데이터가 있음을 알 수 있다. <br>

### The PAN ID Conflict Notification Command Format  PAN 아이디 충돌 알림 명령 형식
PAN 코디네이터가 자체 네트워크 구축을 시작할 때, 먼저 채널 스캔을 수행하여 다른 네트워크를 찾는다. <br>
그런 다음 PAN 코디네이터는 근처의 다른 네트워크에서 사용하지 않는 PAN 식별자(PAN ID)를 선택한다. <br>
그러나, PAN 코디네이터는 그것의 네트워크를 구축한 후, POS에 동일한 PAN ID를 가진 다른 네트워크가 있다는 것을 발견할 수도 있다.<br>
그러한 경우, PAN 코디네이터는 이 문제를 해결하기 위해 PAN ID 충돌 해결 단계를 따를 것이다.<br>
PAN 코디네이터가 근처에 동일한 PAN ID를 가진 다른 네트워크의 존재를 인식할 수 있는 두 가지 방법이 있다.<br>
<br>
1. PAN 코디네이터는 PAN ID가 같은 다른 코디네이터로부터 비콘 프레임을 받는다. 
비콘 프레임에서, PAN 코디네이터 서브필드가 1로 설정되어 있으면, 비콘이 PAN 코디네이터로부터 오고 있는 것을 나타낸다.<br>
<br>
2. PAN 코디네이터와 연결된 장치는 동일한 PAN ID를 가진 다른 네트워크의 존재를 확인하고, <br>
PAN ID 충돌 알림 명령을 사용하여 충돌에 대해 자신의 PAN 코디네이터에게 통보한다. <br>
장치는 장치가 연결된 PAN과 동일한 PAN ID를 가진 PAN 코디네이터로부터 비컨 프레임을 수신하면,<br>
PAN ID는 충돌하지만, 코디네이터 주소가 MAC PIB에 있는 장치와 일치하지 않는다고 결론짓는다. <br>
PAN ID 충돌 알림을 전송한 후, 장치가 PAN 코디네이터로부터 확인응답을 수신하는 경우, <br>
장치 MLME는 MLME-SYNC-LOSS.indication 프리미티브를 사용하여, 
PAN ID 충돌로 인해 동기화가 손실되었음을 NWK 레이어에 통지한다.<br>
<br>
PAN 코디네이터가 이들 방법 중 하나를 통해 PAN ID 충돌을 검출한 후, <br>
능동 스캔을 수행하여 기존 PAN ID를 식별하고, 새롭고 고유한 PAN ID를 선택할 수 있다. <br>
그런 다음 PAN 코디네이터는 이에 따라 수퍼프레임 구성을 업데이트한다.<br>
<br>

###  The Orphan Notification and Beacon Request Command Formats  고아 알림과 비콘 요청 명령 형식
고아 통지 명령의 MAC 페이로드에는 명령 프레임 식별자(그림 3.27)만 포함되며, 고아 통지 절차의 일부로 사용된다. <br>
비콘 요청 명령은 RFD는 선택 사항이며, 장치의 POS 내에서 모든 코디네이터를 찾는 데 사용된다. <br>
MAC 페이로드에는 명령 프레임 식별자만 포함되어 있다(그림 3.27 ) <br>
<br>

###  The Coordinator Realignment and GTS Request Command Formats 코디네이터 재배치와 GTS요청 명령 형식

![image](https://user-images.githubusercontent.com/38284141/52711368-fb779080-2fd4-11e9-832b-0ee58cccad4b.png)<br>

코디네이터는 네트워크 설정의 수정이 필요한 네트워크 변경이 있을 때마다 재배치 명령(그림 3.28a)을 사용한다. <br>
예를 들어, PAN ID 충돌이 있는 경우, PAN 코디네이터는 새롭고 고유한 PAN ID를 선택할 수 있다. <br>
이 경우, PAN 코디네이터는 재배치 명령을 사용하여 새 PAN ID를 네트워크에 있는 장치에 전달한다. <br><br>

GTS 할당 및 할당 해제는 GTS 요청 명령(그림 3.28b)을 사용하여 수행한다. <br>
이 명령은 RFD와 FFD 모두에 대해 선택적이다. GTS 방향 필드는 수신 전용 또는 송신 전용으로 설정할 수 있다. <br>
이 특성 유형을 1로 설정하면, 이 명령을 사용하여 GTS를 할당한다. 0으로 설정하면 GTS에 할당 취소에 명령을 사용한다.<br>

###  The MAC Promiscuous Mode of Operation 맥의 비규칙적인 운영방식
기기가 비규칙적인 작동 모드에 있을 때, 수신하고자 하는 수신자에 관계없이 수신하는 모든 데이터를 읽게 된다. <br>
이 모드는 일반적으로 네트워크 모니터링에 사용된다. <br>
장치는 macPromiscuousMode 속성을 TRUE로 설정하여 비규칙 작동 모드로 전환할 수 있다. <br>
