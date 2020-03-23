TCP & UDP
---------

![image](https://user-images.githubusercontent.com/56240505/72355380-b13de880-372a-11ea-8b22-b78251a42a35.png)<br><br>

-	TCP/IP 프로토콜은 이기종 시스템간의 통신을 위한 표준 프로토콜로, 프로토콜의 집합이다.<br><br>
-	TCP와 UDP 모두 TCP/IP 프로토콜에 속하며, OSI 7계층의 전송 계층에 해당하는 프로토콜이다.<br><br>
-	두 프로토콜은 전송 방식이 다르며, 장단점이 있다.<br><br>

TCP(Transmission Control Protocol)
----------------------------------

-	Layer 4 계층 프로토콜이다.<br><br>
-	TCP는연결 지향 서비스(Connection-Oriented Service)가 제공되므로 데이터에 대한 무결성이 보장되며 양방향 데이터 전송이 가능한 전이중 서비스(Full Duplex Service)를 제공한다.<br><br>
-	스트림 데이터 서비스(Strem Data Service)가 제공되기 때문에 상위 계층으로부터 수신한 데이터 스트림을 세그먼트(Segment)형식의 단위로 분리하여 네트워크로 전송하며 목적지에서는 TCP에 의해서 세그먼트를 재조립한다.<br><br>
-	만약 전송된 세그먼트에 대한 응답을 정해진 타이머 안에 수신하지 못하면 해당 세그먼트를 재전송함으로 신뢰적인 서비스(Reliability Service)를 제공한다.<br><br>

Connection-Oriented Service
---------------------------

![image](https://user-images.githubusercontent.com/56240505/77321322-60afc080-6d55-11ea-8956-8252cdb2ea2a.png)<br><br>

-	TCP는 통신 시 서로의 신뢰 관계를 맽은 후 통신을 하기 때문에 데이터의 무결성을 보장받을 수 있다.<br><br>
-	3-way Handshaking 과정<br>
	-	1. Client는 Server에게 통신 요청을 개시하는 SYN와 첫번째 세그먼트를 알리는 Seq(100)을 전송한다.<br>
	-	2. SYN을 수신한 Server는 이에 대한 응답으로 Client에게 Ack(101)와 Seq(200)을 전송하며, 자기 자신도 Client에게 통신 요청을 개시하는 SYN을 함께 전송한다.<br>
	-	3. SYN을 수신한 Client는 이에 대한 응답으로 Server에게 Ack(201)를 전송함으로써 통신 연결이 성립되게 된다.<br><br>

Full Duplex Service
-------------------

![image](https://user-images.githubusercontent.com/56240505/77322374-fbf56580-6d56-11ea-9ff9-b6d0d63af141.png)<br><br>

-	정보의 송신과 수신을 동시에 행하는 것이 가능하다.<br><br>

-	반이중 서비스는 정보가 송신 및 수신될 때 동시에 수신 및 송신이 이루어지지 않는다.<br><br>

Strem Data Service
------------------

![image](https://user-images.githubusercontent.com/56240505/77322595-4ecf1d00-6d57-11ea-8feb-d92549b1f255.png)<br><br>

-	데이터를 세그먼트 형식의 단위로 분리하여 전송한다.<br><br>

Reliability Service
-------------------

![image](https://user-images.githubusercontent.com/56240505/77322785-99e93000-6d57-11ea-80fa-2789080092a5.png)<br><br>

-	TCP는 세그먼트에 포함된 체크섬을 이용하여 세그먼트 손상 여부를 검사한다.<br><br>
-	만약 세그먼트 손상이 발견 된다면 수신측에서는 해당 세그먼트를 폐기하고 송신측으로 응답을 보내지 않는다.<br><br>
-	또한 송신측은 일정 시간이 초과되기 이전에 응답을 수신하지 못하면 해당 세그먼트가 손상되었거나 손실되었다고 간주한다.<br><br>
-	이런 경우 송신측에서는 전송되는 각 세그먼트마다 시간 초과 카운트를 이용하여 주기적으로 확인하다가 카운터값이 만기되면 해당 세그먼트를 재전송한다.<br><br>

Flow Control
------------

![image](https://user-images.githubusercontent.com/56240505/77323390-84c0d100-6d58-11ea-8033-a98a528ebb40.png)<br><br>

-	통신에 참여한 두 호스트가 송수신할 수 있는 데이터 양을 네트워크 상황에 맞게 조절하는 기능을 제공한다.<br><br>
-	Stol-and wait : 일반적으로 데이터를 전송하기 이전에 송신한 데이터에 대한 응답을 먼저 수신해야지만 그 다음 데이터를 전송할 수 있다.<br><br>
-	TCP는 슬라이딩 윈도우(Sliding Window) 기능을 이용하여 상호 협의한 윈도우 크기만큼 데이터를 송신하고 이에 대한 응답만 수신하면 그 다음 윈도우 크기만큼 데이터를 전송한다.<br><br>

Congestion Control
------------------

-	과도한 패킷이 들어와 라우터의 버퍼 용량을 초과하게 될 때 혼잡이 발생하며 패킷들이 손실되게 된다.<br><br>
-	이러한 문제를 해결하기 위해서 혼잡 제어 기능을 이용하여 송신측에서 패킷을 보내는 전송율을 제어한다.<br><br>

TCP 프로토콜을 사용하는 어플리케이션 프로토콜
---------------------------------------------

-	HTTP(Port 80)
-	TELNET(Port 23)
-	SSH(Port 22)
-	FTP(Port 20/21)
-	SMTP(Port 25)
-	POP(Port 110)<br><br>

UDP(User Datagram Protocol)
---------------------------

-	Layer 4 계층 프로토콜이다.<br><br>
-	UDP는 비연결 지향 서비스(Connectionless Service)가 제공되므로 송신측과 수신측간에 접속 절차를 거치지 않고 수신측에서 요청을 실시하면 송신측에서 바로 데이터를 전송한다.<br><br>
-	또한 TCP에서 통신 연결과 제어를 담당하는 3-way Handshaking, 데이터 스트림 서비스, 재전송 서비스, 혼잡 제어 기능을 수행하지 않는다.<br><br>

Best Effort Service
-------------------

-	UDP는 최선을 다해 목적지까지 데이터를 전송하지만 신뢰성을 보장하는 서브사가 없기 때문에 이를 어플리케이션 계층에서 해결해야 한다.<br><br>
-	대신 신뢰성을 보장하는 서비스로 인한 데이터 전송 지연이 발생하지 않기 때문에 신속한 데이터 전송이 가능하며 이러한 특징 때문에 실시간으로 트래픽을 전송하는 멀티미디어 서비스에 주로 사용한다.<br><br>

UDP 프로토콜을 사용하는 경우
----------------------------

-	단순한 데이터 요청과 응답을 하는 경우.<br><br>
-	오류 제어가 크게 필요하지 않은 경우.<br><br>
-	내부적으로 흐름 제어와 오류 제어 매커니즘을 갖고 있는 경우.<br><br>
-	멀티캐스트와 브로드케스트 서비스.<br><br>

UDP Header
----------

-	①, ② : 출발지, 목적지에서 수행되는 프로세스가 사용하는 포트 번호이다.<br><br>

-	③ : 헤더와 데이터를 합한 사용자 데이터그램의 전체 길이를 정의하는 필드이다.<br><br>

-	④ : 사용자 데이터그램에 대한 오류 검사를 담당하는 필드로서, 오류가 확인되면 재전송하지 않고 그냥 폐기한다.<br><br>

-	이처럼 UDP는 신뢰성을 보장하는 서비스 기반이 아니기 때문에 TCP 헤더에 비해서 상당히 간략한 편이다.<br><br>

UDP 프로토콜을 사용하는 어플리케이션 프로토콜 및 라우팅 프로토콜
----------------------------------------------------------------

-	DNS(Port 53)
-	DHCP(Port 67/68)
-	TFTP(Port 69)
-	SNMP(Port 161)
-	NTP(Port 123)
-	RIP(Port 520)<br><br>

---

Reference
---------

-	[TCP?](https://m.blog.naver.com/hatesunny/220790242308)
-	[UDP? UDP 헤더](https://m.blog.naver.com/hatesunny/220790304526)
