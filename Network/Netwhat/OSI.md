OSI(Open System Interconnection)
--------------------------------

-	OSI 참조 모델은 컴퓨터 응용 프로그램 정보가 어떤 과정을 통해 다른 컴퓨터 응용 프로그램으로 이동하는지 설명한다.<br><br>
-	OSI 참조 모델은 상위 계층과 하위 계층으로 구분되며, 세부적으로는 7 계층으로 구분된다.<br><br>
-	호환성 : OSI 참조 모델은 네트워크 설계를 하기 위한 프레임워크를 제공한다.<br><br>
-	트러블 슈팅 : 네트워크 장애 발생시 문제 해결 방법을 체계적으로 접근하기 위해 필요하다.<br><br>

Upper Layer
-----------

![image](https://user-images.githubusercontent.com/56240505/77377463-7dc8ab80-6db6-11ea-8550-52d27e35c67b.png)<br><br>

-	상위 계층은 데이터(PDU, Protocol Data Unit) 생성을 담당한다.<br><br>
-	일반적으로 소프트웨어로 구현되고 OS가 그 일을 담당한다.<br><br>

Lower Layer
-----------

![image](https://user-images.githubusercontent.com/56240505/77377485-891bd700-6db6-11ea-87dd-d00a53a66b2d.png)<br><br>

-	하위 계층은 데이터 전송을 담당한다.<br><br>

OSI 참조 모델을 이용한 데이터 처리 과정
---------------------------------------

![image](https://user-images.githubusercontent.com/56240505/77377613-f4fe3f80-6db6-11ea-9f28-005c16fea928.png)<br><br>

-	OSI 7 계층은 여러 형식의 제어 정보를 이용하여 다른 컴퓨터 시스템에 있는 동일한 계층간에 통신을 실시한다.<br><br>
-	이 때 제어 정보에는 서비스 요청 내용과 서비스 명령 내용이 포함되어 있으며, 이는 동일한 계층 사이에만 교환된다.<br><br>
-	송신측의 각 계층에서는 수신측의 동일한 계층에서 필요한 정보를 공통된 서식으로 추가하기 위하여 상위 계층에서 넘겨 받은 데이터 앞에 헤더를 덧붙인다.<br><br>
-	이러한 동작이 하위 계층으로 넘어가면서 헤더는 데이터와 합쳐지며, 또 하나의 새로운 데이터 형식을 만들기 위해 캡슐화 과정을 실시한다.<br><br>
-	상위 계층에서 하위 계층으로 처리될 때마다 새로운 데이터 형식으로 캡슐화하기 때문에, 서로 다른 계층 간에 데이터 내용은 확인할 수 없다.<br><br>
-	위의 그림과 같이 서버에서 클라이언트로 데이터를 전송할 경우, 가장 먼저 어플리케이션 계층에서 데이터를 처리한다.<br><br>
-	이 때 서버는 클라이언트 어플리케이션 계층에서 필요한 정보를 PDU라는 형식의 데이터로 생성한다.<br><br>
-	생성된 PDU는 Layer 6 계층에서 Layer 2 계층까지 처리될 때 마다, 각 계층에 필요한 헤더가 추가된다.<br><br>
-	마지막으로 Layer 1 계층에서는 네트워크 인터페이스로 데이터를 출력할 수 있도록 전기 신호 변환을 담당한다.<br><br>
-	서버로부터 데이터를 수신한 클라이언트는 역순의 과정을 실시하여 상위 계층까지 서비스가 도달하게 된다.<br><br>

Network Layer
-------------

-	OSI 참조 모델 중 Layer 3 계층이며, 데이터를 목적지까지 가장 안전하고 빠르게 전달하는 라우팅을 통해 패킷 포워딩을 담당한다.<br><br>
-	주소 부여(IP) 및 경로 설정(Route) 역할을 한다.<br><br>
-	사용하는 프로토콜 및 라우팅 기술이 다양하며, 대표적인 장비로는 라우터가 있다.<br><br>
-	네트워크 계층은 경로를 선택하고 주소를 정하고, 경로에 따라 패킷을 전달한다.<br><br>
-	네트워크들을 통해 다양한 길이의 데이터를 전달하고 그 과정에서 전송 계층이 요구하는 서비스 품질을 제공하기 위한 기능적 및 절차적 수단을 제공한다.<br><br>
-	네트워크 계층은 라우팅, 흐름 제어, 세그멘테이션, 오류 제어, 인터네트워킹 등을 수행한다.<br><br>
-	연결된 다른 네트워크를 통해 데이터를 전달함으로써 인터넷이 가능하게 만드는 계층이다.<br><br>
-	논리적인 주소 구조(IP), 즉 네트워크 관리자가 직접 주소를 할당하는 구조를 가지며 계층적이다.<br><br>
-	서브네트의 최상위 계층으로 경로를 설정하고, 청구 정보를 관리한다.<br><br>
-	개방형 시스템들의 사이에서 네트워크 연결을 설정, 유지, 해제하는 기능을 부여하고 전송 계층 사이에서 네트워크 서비스 데이터 유닛(NSDU, Network Service Data Unit)을 교환하는 기능을 제공한다.<br><br>

---

Reference
---------

-	[OSI 참조 모델](https://m.blog.naver.com/hatesunny/220789832663)
-	[OSI 7 계층이란?, OSI 7 계층을 나눈 이유](https://shlee0882.tistory.com/110)
