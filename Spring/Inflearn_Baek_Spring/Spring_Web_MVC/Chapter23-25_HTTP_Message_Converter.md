HTTP Message Converter
----------------------

> SimpleController.java

```java
@GetMapping("/message")
@ResponseBody
public String message(@RequestBody String body) {
		return body;
}
```

<br>

> SimpleControllerTest.java

```java
@Test
public void message() throws Exception {
		this.mockMvc.perform(get("/message").content("hello"))
								.andDo(print())
								.andExpect(status().isOk())
								.andExpect(content().string("hello"));
}
```

-	요청 본문에서 메시지를 읽어들이거나(@RequestBody), 응답 본문에 메시지를 작성할 때(@ResponseBody) 사용한다.<br><br>
-	@RestController를 사용하면 @ResponseBody를 생략할 수 있다.<br><br>
-	요청 본문의 메시지를 특정 자료형으로 Conversion하며, 다시 응답의 Body에 담아 보내는 일련의 로직이다.<br><br>
-	Request의 Header 타입에 따라 어떤 컨버터를 쓸 지 결정된다.<br><br>

기본 HTTP Message Converter
===========================

-	바이트 배열 컨버터.<br><br>
-	문자열 컨버터.<br><br>
-	Resource 컨버터.<br><br>
-	Form 컨버터 (폼 데이터 to/from MultiValueMap<String, String>).<br><br>
-	(JAXB2 컨버터).<br><br>
-	(Jackson2 컨버터).<br><br>
-	(Jackson 컨버터).<br><br>
-	(Gson 컨버터).<br><br>
-	(Atom 컨버터).<br><br>
-	(RSS 컨버터).<br><br>
-	...<br><br>

설정 방법
---------

-	기본으로 등록해주는 컨버터 목록에 새로운 컨버터 추가하기 : extendMessageConverters.<br><br>
-	기본으로 등록해주는 컨버터 목록은 다 무시하고 새로 컨버터 설정하기 : configureMessageConverters.<br><br>

-	의존성 추가로 컨버터 등록하기

	-	메이븐 또는 그래들 설정에 의존성을 추가하면 그에 따른 컨버터가 자동으로 등록 된다.

	-	WebMvcConfigurationSupport

	-	이 기능 자체는 스프링 프레임워크의 기능이다.<br><br>

JSON Converter
--------------

> SimpleController.java

```java
@GetMapping("/jsonmessage")
public Person jsonMessage(@RequestBody Person person) {
		return person;
}
```

<br>

> SimpleControllerTest.java

```java
@Autowired
ObjectMapper objectMapper;

@Test
public void jsonMessage() throws Exception {
		Person person = new Person();
		person.setId(2019l);
		person.setName("jipark");
		String jsonString = objectMapper.writeValueAsString(person);
		this.mockMvc.perform(get("/jsonmessage")
						.contentType(MediaType.APPLICATION_JSON)
						.accept(MediaType.APPLICATION_JSON)
						.content(jsonString))
						.andDo(print())
						.andExpect(status().isOk())
						.andExpect(jsonPath("$.id").value(2019))
						.andExpect(jsonPath("$.name").value(jipark));
}
```

-	Spring Boot를 사용하지 않는 경우.
	-	사용하고 싶은 JSON 라이브러리를 의존성으로 추가한다.
	-	GSON.
	-	JacksonJSON.
	-	JacksonJSON 2.<br><br>
-	Spring Boot를 사용하는 경우 기본적으로 JacksonJSON 2가 의존성에 들어있다.<br><br>
-	JSON path로 값의 비교가 더욱 명확하게 가능하다.<br><br>

XML Converter
-------------

> pom.xml

```xml
<dependency>
	<groupId>javax.xml.bind</groupId>
	<artifactId>jaxb-api</artifactId>
</dependency>
<dependency>
	<groupId>org.glassfish.jaxb</groupId>
	<artifactId>jaxb-runtime</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-oxm</artifactId>
	<version>${spring-framework.version}</version>
</dependency>
```

<br>

> WebConfig.java

```java
@Bean
public Jaxb2Marshaller marshaller() {
		Jaxb2Marshaller jaxb2Marshaller = new Jaxb2Marshaller();
		jaxb2Marshaller.setPackagesToScan(Person.class.getPackageNames());
		return jaxb2Marshaller;
}
```

<br>

> Person.java

```java
@XmlRootElement
```

<br>

> SimpleControllerTest.java

```java
@Autowired
Marshaller marshaller;

@Test
public void xmlMessage() throws Exception {
		Person person = new Person();
		person.setId(2019l);
		person.setName("jipark");
		StringWriter stringWriter = new StringWriter();
		Result result = new StreamResult(stringWriter);
		marshaller.marshal(person, result);
		String xmlString = stringWriter.toString();
		this.mockMvc.perform(get("/jsonmessage")
						.contentType(MediaType.APPLICATION_XML)
						.accept(MediaType.APPLICATION_XML)
						.content(xmlString))
						.andDo(print())
						.andExpect(status().isOk())
						.andExpect(xpath("person/name").string("jipark"));
}
```

-	Spring Boot는 기본적으로 XML 의존성을 추가해주지 않는다.<br><br>
-	OXM(Object-XML Mapper) 라이브러리 중 Spring이 지원하는 JacksonXML 및 JAXB 등의 의존성을 추가한다.<br><br>
-	특정 Root Element를 인지해야 XML 변환이 가능하다.<br><br>
-	JSON 예시처럼 헤더 정보를 그에 맞게 명시적으로 수정하고. Path를 확인할 수 있다.<br><br>

---

Reference
---------

-	스프링 웹 MVC(백기선, Inflearn)
