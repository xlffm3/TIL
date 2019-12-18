Spring XML&Config Practice
--------------------------

---

### Bean Class<br>

-	일반적인 Java Class를 Bean Class라고 부른다.<br><br>
-	기본 생성자를 가지고 있으며, 필드는 private하게 선언하며, getter 및 setter 메서드를 가진다.<br><br>
-	getter 및 setter 메서드를 name property라고 부른다.<br><br>
-	Spring은 bean 생성시 기본적으로 싱글톤 패턴로 객체를 생성하며, 메모리에 하나만 생성한다는 뜻이다.<br><br>
-	bean에 scope를 줄 수 있다.<br><br>

### 설정 방법<br>

-	xml 파일 혹은 Java Config를 이용하여 설정할 수 있다.<br><br>
-	각각의 방법들은 Reference 사이트를 참고하자.<br><br>

### XML 설정 후 ApplicationContext를 이용해서 실행하기<br><br>

```java
//applicationContext.XML
<bean id="userBean" class="kr.or.connect.diexam01.UserBean"></bean>

//main
ApplicationContext ac = new ClassPathXmlApplicationContext(
                "classpath:applicationContext.xml");
System.out.println("초기화 완료.");

UserBean userBean = (UserBean)ac.getBean("userBean");
userBean.setName("kim");
System.out.println(userBean.getName());

UserBean userBean2 = (UserBean)ac.getBean("userBean");
if(userBean == userBean2) {
  System.out.println("같은 인스턴스이다.");
```

-	ApplicationContext는 싱글턴 패턴으로서, getBean으로 객체를 계속 요청하더라도 객체를 계속 생성하는 것이 아니라 하나 만든 bean을 계속 이용하는 것이다.<br><br>
-	그렇기 때문에 getBean으로 요청 받은 인스턴스는 동일하다고 인식한다.<br><br>

### DI 주입<br>

```java
//applicationContext.XML
<bean id="e" class="kr.or.connect.diexam01.Engine"></bean>
<bean id="car" class="kr.or.connect.diexam01.Car">
    <property name="engine" ref="e"></property>
</bean>

//main
ApplicationContext ac = new ClassPathXmlApplicationContext(
                "classpath:applicationContext.xml");

        Car car = (Car)ac.getBean("car");
        car.run();
```

-	ref는 특정 bean을 사용하겠다고 선언하는 것이다.<br><br>
-	name이 engine인 property는 getEngine 혹은 setEngine을 의미한다.<br><br>
-	bean tag 안에서는 모두 값을 설정하는 것이기 때문에, id가 e로 선언된 인스턴스를 setEngine 메서드의 파라미터로 전해주는 것을 뜻한다.<br><br>
-	이처럼 객체에 객체를 주입하는 것을 DI라고 한다.<br><br>

### Java Config를 이용한 설정을 위한 Annotation<br>

-	@Configuration : 스프링 설정 클래스를 선언하는 어노테이션.<br><br>
-	@Bean : bean을 정의하는 어노테이션.<br><br>
-	@ComponentScan : @Controller, @Service, @Repository, @Component 어노테이션이 붙은 클래스를 찾아 컨테이너에 등록한다.<br><br>
-	@Component : 컴포넌트 스캔의 대상이 되는 애노테이션 중 하나로써 주로 유틸, 기타 지원 클래스에 붙이는 어노테이션.<br><br>
-	@Autowired : 주입 대상이되는 bean을 컨테이너에 찾아 주입하는 어노테이션.<br><br>

### 참고 코드<br>

```java
//ApplicationConfig
@Configuration
public class ApplicationConfig {
    @Bean
    public Car car(Engine e) {
        Car c = new Car();
        c.setEngine(e);
        return c;
    }

    @Bean
    public Engine engine() {
        return new Engine();
    }
}

//main
ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);

Car car = (Car)ac.getBean("car");
//Car car = (Car)ac.getBean(Car.class); //클래스 타입을 넣어도 가능하다.
car.run();
```

-	ApplicationContext는 파라미터를 받아들이지 않는 bean 생성 메서드를 먼저 다 실행한 뒤, 반환받은 객체를 관리한다.<br><br>
-	그 후, 생성자 파라미터에 명시된 객체와 같은 타입의 객체가 있을 경우에, 파라미터에 전달하여 객체를 생성한다.<br><br>

### ComponentScan 활용<br>

```java
//ApplicationConfig
@Configuration
@ComponentScan("kr.or.connect.diexam01")
public class ApplicationConfig2 {
}

//Car
@Component
public class Car {
    @Autowired
    private Engine v8;

    public Car() {
        System.out.println("Car 생성자");
    }

    public void run() {
        System.out.println("엔진을 이용하여 달립니다.");
        v8.exec();
    }
}
```

-	ComponentScan으로 Component 등 다른 Annotation을 읽어 메모리에 올리며, 조사의 대상이 되는 패키지를 명시해야 한다.<br><br>
-	Autowired를 통해 Engine 타입의 객체가 생성된 것이 있으면 해당 참조변수에 주입한다.<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/20658/)

---
