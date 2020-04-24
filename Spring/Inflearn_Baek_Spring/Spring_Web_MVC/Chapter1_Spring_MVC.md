##MVC(Model-View-Controller)

-	Model : 도메인 객체 또는 DTO로 화면에 전달하거나 화면에서 전달 받은 데이터를 담고 있는 객체를 의미한다.<br><br>
-	View : HTML, JSON, XMl 등 데이터를 보여주는 역할이다.<br><br>
-	Controller : Spring @MVC 등 사용자 입력을 받아 Model 객체의 데이터를 변경하거나 Model 객체를 View에 전달하는 역할이다.<br><br>

MVC 패턴의 장점
---------------

-	동시 다발적(Simultaneous) 개발 : 백엔드 개발자와 프론트엔드 개발자가 독립적으로 개발을 진행할 수 있다.<br><br>
-	높은 결합도 : 논리적으로 관련있는 기능을 하나의 Controller로 묶거나, 특정 Model과 관련있는 View를 그룹화 할 수 있다.<br><br>
-	낮은 의존도 : MVC는 각각 독립적이다.<br><br>
-	개발 용이성 : 책임이 구분되어 있어 코드 수정하는 것이 편하다.<br><br>
-	한 Model에 대한 여러 형태의 View를 가질 수 있다.<br><br>

Lombok 관련 코드
----------------

> Event.java

```java
@Getter @Setter
@Builder @NoArgsConstructor @AllArgsConstructor
```

-	Getter와 Setter 및 Constructor 등을 Annotation 기반으로 자동 생성해준다.<br><br>
-	컴파일 후 Target 디렉토리를 보면 코드들이 추가된 것을 확인할 수 있다.<br><br>
-	@RequestMapping보다 @GetMapping 등이 더 코드가 간결한 확장 버전이다.<br><br>

---

Reference
---------

-	스프링 웹 MVC(백기선, Inflearn)
