Spring MVC
----------

---

<br>

### MVC(Model-View-Controller)<br><br>

> -	원래는 제록스 연구소에서 일하던 트뤼그베 린즈커그가 처음으로 소개한 개념으로, 데스트톱 어플리케이션용으로 고안되었다.<br><br>
> -	**Model** : 모델은 뷰가 렌더링하는데 필요한 데이터이며, 예를 들어 사용자가 요청한 상품 목록이나 주문 내역이 이에 해당한다.<br><br>
> -	**View** : 웹 애플리케이션에서 뷰(View)는 실제로 보이는 부분이며, 모델을 사용해 렌더링을 하며, 뷰는 JSP, JSF, PDF, XML등으로 결과를 표현한다.<br><br>
> -	**Controller** : 컨트롤러는 사용자의 액션에 응답하는 컴포넌트이며, 컨트롤러는 모델을 업데이트하고, 다른 액션을 수행한다.

<br><br>

### MVC Model 1 Architecture<br><br>

![image](https://user-images.githubusercontent.com/56240505/70532258-04f06b80-1b9a-11ea-9add-280ec58a252d.png)<br><br>

> -	JSP 자체에 JAVA 코드와 HTML 코드가 섞여 유지 보수가 어렵다.

<br><br>

### MVC Model 2 Architecture<br><br>

![image](https://user-images.githubusercontent.com/56240505/70532518-8ea03900-1b9a-11ea-8850-a78ebf61660f.png)<br> ![image](https://user-images.githubusercontent.com/56240505/70532646-d1faa780-1b9a-11ea-8c5e-2e4fc37ac5ce.png)<br><br>

> -	로직과 뷰를 분리한다.<br><br>
> -	클라이언트의 요청은 Front Controller Servlet이 받으며, Front Controller는 단 1개만 존재한다.<br><br>
> -	Front Controller는 Controller Class(a.k.a Handler Class)에 요청을 위임한다.<br><br>
> -	Servlet은 관련 요청을 처리하기에 구조가 다소 불편하기 때문에, 실제 처리는 위임함으로써 관련된 URL을 하나의 클래스에서 다 처리할 수 있도록 한다.<br><br>
> -	만들어진 결과를 모델에 담아 Front Controller에 보내면, 알맞은 뷰에 이를 전달하여 결과를 출력한다.

<br><br>

### Spring Web Module<br><br>

![image](https://user-images.githubusercontent.com/56240505/70533016-8694c900-1b9b-11ea-91e9-d2615f27875f.png)<br><br>

> -	Model 2의 발전된 형태가 Spring Framework의 Web Module에 구현되어 있으며, 이를 보통 Spring MVC라고 한다.

<br><br>

### Front Controller로 얻는 이점<br><br>

> -	하나의 웹 어플리케이션은 많은 뷰와 컨트롤러가 존재하는데, 각각의 뷰와 컨트롤러가 독립적으로 연결되면 서버 입장에서는 웹 어플리케이션 실행에 대해 일괄적으로 처리하기 어렵다.<br><br>
> -	**Front Controller Design Pattern** : Front Controller를 두고 뷰에서 들어오는 모든 요청을 담당하여 웹 어플리케이션을 실행하는 모든 요청을 일괄적으로 처리하는 구조이다.

<br><br>

[Reference]

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16762/)

---
