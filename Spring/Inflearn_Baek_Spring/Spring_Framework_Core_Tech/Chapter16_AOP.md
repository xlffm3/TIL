Chapter 16 : AOP(Aspect-Oriented Programming)
=============================================

AOP(Aspect-Oriented Programming)
--------------------------------

![image](https://user-images.githubusercontent.com/56240505/80103754-60068600-85b0-11ea-9cd7-673db96e6c32.png)<br><br>

-	OOP를 보완하는 수단으로서, 흩어진 Aspect를 모듈화 할 수 있는 프로그래밍 기법이다.<br><br>
-	주요 개념.
	-	Aspect : 모듈.
	-	Advice : 해야할 일들.
	-	Target : Advice가 적용이 되는 대상(Class A, B, C 등).
	-	Joint Point : 생성자가 호출되었을 때 혹은 필드에서 값을 가져왔을 때 등 Advice의 여러가지 합류 지점과 같은 Spec.
	-	Pointcut : Joint Point의 Subset으로서 특정 클래스의 특정 메서드가 호출될 때 Advice를 적용한다 등의 구체적인 정보.<br><br>

AOP 적용 방법
-------------

-	컴파일 : Java 파일을 Class 파일로 변환할 때 바이트 코드가 조작이 된 파일을 생성한다.<br><br>
-	로드 타임 : 순수하게 컴파일을 한 뒤, 특정 Class 파일을 로딩하는 시점에 Class 파일 정보를 변경하는 로드 타임 위빙을 통해 적용한다.<br><br>
-	런타임 : Bean을 생성할 때 해당 Bean의 Proxy Bean을 생성하여 Advice를 수행하는 AOP를 적용한다.<br><br>
-	컴파일 및 로드 타임은 AspectJ를, 런타임에는 Spring AOP를 이용한다.<br><br>
-	컴파일 및 로드 타임에서의 AOP 설정은 별도의 추가 설정을 요구한다.<br><br>
-	각 적용 방법에 따라 특정 시점에서의 성능 저하가 유발될 수 있다.<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
