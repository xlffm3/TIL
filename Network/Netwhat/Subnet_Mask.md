Netmask
-------

-	하나의 네트워크를 몇 개의 네트워크로 나누어 사용할 때, 나눠진 각각의 네트워크를 구분하기 위해 사용하는 특수한 bit이다.<br><br>
-	IP 주소에 대한 네트워크 아이디와 호스트 아이디를 구분하기 위해서 사용된다.<br><br>
-	IPv4 주소의 고갈이 점차 현실화되면서 이를 최대한 늦추기 위하여, 각국의 NIC에서는 각 라우터가 브로드캐스팅하는 로컬 네트워크 영역에 공인 IP 대역을 호스트가 필요한 만큼만 할당하려는 노력을 했다.<br><br>
-	이러한 NIC 기관의 요구에 맞춰서 IETF에서는 로컬 네트워크 내부에서 접속한 호스트의 IP 대역을 외부 네트워크와 명확하게 구분할 수 있는 수단을 표준화하였으며, 이것이 서브넷 마스크(Subnet Mask)이다.<br><br>

Netmask 표시 방법
-----------------

![image](https://user-images.githubusercontent.com/56240505/77307442-985e3e80-6d3c-11ea-991a-81d1b79e8546.png)<br><br>

-	IP 주소는 네트워크 부분과 호스트 부분으로 나누어진다.<br><br>
-	하나의 로컬 네트워크란 하나의 라우터를 거쳐가는 여러 개의 호스트들이 연결된 브로드캐스트 영역이다.<br><br>
-	즉, 어떤 네트워크에서 한 노드가 브로드캐스트를 했을 때 그 네트워크의 모든 노드가 신호를 받았다면 그 네트워크는 하나의 네트워크라고 볼 수 있다.<br><br>
-	다시 말해, 하나의 로컬 네트워크에서는 IP 주소의 네트워크 부분은 같아야 하고, 호스트 부분은 달라야 한다.<br><br>
-	서브넷 마스크의 맨 앞 bit부터 1이 연속된 구간까지를 공통 bit로 처리하여 네트워크 아이디로 사용하며, 0으로 끝나는 마지막 구간까지를 공통하지 않는 bit로 처리하여 호스트 아이디로 사용한다.<br><br>
-	255.255.255.0은 /24로 표기할 수 있는데, 이는 서브넷 마스크 맨 앞의 bit부터 1의 개수를 표기하는 Prefix 표기 방법을 따른다.<br><br>
-	서브넷 마스크를 계산할 때는 논리곱(논리 AND)를 사용한다.<br><br>
-	네트워크 대역의 호스트 개수를 계산할 때, 네트워크 주소와 브로드캐스트 주소를 제외한다.<br><br>

Subnetting
----------

![image](https://user-images.githubusercontent.com/56240505/77375621-66d38a80-6db1-11ea-9981-822a92074ae4.png)<br><br> ![image](https://user-images.githubusercontent.com/56240505/77375746-c467d700-6db1-11ea-917a-cff6a7d8f2c4.png)<br><br> ![image](https://user-images.githubusercontent.com/56240505/77375769-d34e8980-6db1-11ea-8071-14e0c39f18a2.png)<br><br>

-	클래스 A, B, C TCP/IP 네트워크는 시스템 관리자에 의해 더 작은 단위의 네트워크, 즉 서브넷으로 나누어질 수 있다.<br><br>

-	호스트 주소에 사용되는 bit 중 일부를 '빌려', 이를 주소의 네트워크 부분에 사용한다.<br><br>

-	네트워크에 존재하는 호스트 수가 10개인데 C Class를 할당받는다면 나머지 246개는 낭비되는 등의 이슈가 있기 때문에 서브넷팅을 실시한다.<br><br>

-	위 그림을 통해 서브넷은 총 5개가 필요하며, 호스트는 최대 29개임을 확인할 수 있다.<br><br>

-	네트워크 아이디와 서브넷 브로드캐스트 주소를 제외하여 도출한 2^X - 2 >= 29 식을 통해, X의 적정 값이 5임을 도출할 수 있다.<br><br>

-	호스트의 5 bit를 제외한 나머지 3 bit 부분을 전부 1로 채워, 총 8개의 서브넷으로 분리가 됨을 확인할 수 있다.<br><br>

Subnetting 계산 예제
--------------------

![image](https://user-images.githubusercontent.com/56240505/77314045-48857480-6d48-11ea-8c3a-7d2ec604d431.png)<br><br>

-	서브넷 마스크 255.255.255.192는 각각 62대의 호스트로 이루어진 4개의 네트워크에 사용할 수 있다.<br><br>

-	이는 네트워크 주소는 늘리고, 가능한 한 호스트 주소 범위는 줄이는 서브넷 마스크를 사용하여서 네트워크를 4개의 서브넷으로 나눴다.<br><br>

-	이진 표기법에서는 255.255.255.192가 11111111.11111111.11111111.11000000 과 같기 때문에 그렇다.<br><br>

-	마지막 옥텟의 처음 두 자릿수는 네트워크 주소가 되어 추가 네트워크 00000000(0), 01000000(64), 10000000(128), 11000000(192)를 가질 수 있다.<br><br>

-	모두 1과 0으로 이루어진 이진 호스트 주소는 유효하지 않으므로 마지막 옥텟이 0, 63, 64, 127,128, 191, 192, 255인 주소는 사용할 수 없다.<br><br>

-	C Class 서브넷 마스크 255.255.255.0을 사용할 경우, 192.168.123.71과 192.168.123.133 두 주소 모두 192.168.123.0 네트워크에 있다.<br><br>

-	그러나 255.255.255.192를 사용할 경우, 192.168.123.71은 192.168.123.64 네트워크에 있고 192.168.123.133은 192.168.123.128 네트워크에 있게 된다.<br><br>

-	이러한 경우 브로드캐스트 주소는 다음과 같은 방식으로 구한다.<br>

	-	1. Subnet Mask를 invert한다.
	-	2. Invert한 Subnet Mask와 IP address를 Logical OR를 한다.<br><br>

---

Reference
---------

-	[IP 주소 및 서브넷마스크](https://m.blog.naver.com/PostView.nhn?blogId=hatesunny&logNo=220790654612&proxyReferer=https%3A%2F%2Fwww.google.com%2F)
-	[서브넷팅](https://m.blog.naver.com/hatesunny/220792560549)
