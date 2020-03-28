Multipart
---------

![image](https://user-images.githubusercontent.com/56240505/71478449-193b9600-2833-11ea-8fb4-e21955ed4314.png)<br><br>

-	웹 클라이언트가 요청을 보낼 때 HTTP 프로토콜의 바디 부분에 데이터를 여러 부분으로 나눠서 보내는 것이며, 주로 파일 전송에 사용된다.<br><br>
-	HttpServletRequest는 웹 클라이언트가 전달하는 Multipart 데이터를 쉽게 처리하는 메소드를 제공하지 않는다.<br><br>
-	서블릿에서 파일 업로드를 처리하려면 별도의 라이브러리를 사용해야 한다.<br><br>
-	대표적인 라이브러리가 아파치 재단의 commons-fileupload이다.<br><br>

Spring MVC 설정
---------------

> commons-fileupload, commons-io 라이브러리 추가

```xml
<dependency>
<groupId>commons-fileupload</groupId>
<artifactId>commons-fileupload</artifactId>
<version>1.2.1</version>
</dependency>
<dependency>
<groupId>commons-io</groupId>
<artifactId>commons-io</artifactId>
<version>1.4</version>
</dependency>
```

<br>

> MultipartResolver Bean 추가

```java
@Bean
public MultipartResolver multipartResolver() {
  org.springframework.web.multipart.commons.CommonsMultipartResolver multipartResolver
    = new org.springframework.web.multipart.commons.CommonsMultipartResolver();
  multipartResolver.setMaxUploadSize(10485760); // 1024 * 1024 * 10
  return multipartResolver;
}
```

-	DispathcerServlet은 준비 과정에서 "multipart/form-data"가 요청으로 올 경우 MultipartResolver를 사용한다.<br><br>

Upload 처리
-----------

-	파일 업로드 시에는 form태그에 enctype설정이 되어 있어야 하며, post 방식이어야 한다.<br><br>

-	Controller는 @PostMapping이 사용되야 한다.<br><br>

-	업로드 파일이 하나일 경우 `@RequestParam("file") MultipartFile file` 로 표기한다.<br><br>

-	업로드 파일이 여러 개일 경우 `@RequestParam("file") MultipartFile[] files` 로 표기한다.<br><br>

-	MultipartFile의 메소드를 이용해서 파일 이름, 파일 크기 등을 구하고 InputStream을 얻어 파일을 서버에 저장한다.<br><br>

Download 처리
-------------

```java
response.setHeader("Content-Disposition", "attachment; filename=\"" + fileName + "\";");
response.setHeader("Content-Transfer-Encoding", "binary");
response.setHeader("Content-Type", contentType);
response.setHeader("Content-Length", fileLength;
response.setHeader("Pragma", "no-cache;");
response.setHeader("Expires", "-1;");
```

-	서버의 특정 디렉토리는 외부에서 접근할 수 없기 때문에, 파일을 외부에서 사용하려면 다운로드 기능이 필요하다.<br><br>
-	파일 다운로드와 관련된 헤더 정보 및 캐시 미사용 등 설정을 추가한다.<br><br>
-	파일을 읽어 HttpServletResponse의 OutputStream으로 출력한다.<br><br>

Upload Practice
---------------

```java
@PostMapping("/upload")
public String upload(@RequestParam("file") MultipartFile file) {

  System.out.println("파일 이름 : " + file.getOriginalFilename());
  System.out.println("파일 크기 : " + file.getSize());

      try(
              // 맥일 경우
              //FileOutputStream fos = new FileOutputStream("/tmp/" + file.getOriginalFilename());
              // 윈도우일 경우
              FileOutputStream fos = new FileOutputStream("c:/tmp/" + file.getOriginalFilename());
              InputStream is = file.getInputStream();
      ){
            int readCount = 0;
            byte[] buffer = new byte[1024];
          while((readCount = is.read(buffer)) != -1){
              fos.write(buffer,0,readCount);
          }
      }catch(Exception ex){
          throw new RuntimeException("file Save Error");
      }

  return "uploadok";
}
```

-	파일명 중복 및 한 디렉토리에 저장될 수 있는 파일의 수 제한 등 문제가 발생할 수 있다.<br><br>
-	파일이 업로드 되는 날짜 등을 이용하여 파일명과 디렉토리 구조를 적절히 변경하여 이를 극복한다.<br><br>

Download Practice
-----------------

```java
@GetMapping("/download")
public void download(HttpServletResponse response) {

      // 직접 파일 정보를 변수에 저장해 놨지만, 이 부분이 db에서 읽어왔다고 가정한다.
  String fileName = "connect.png";
  String saveFileName = "c:/tmp/connect.png"; // 맥일 경우 "/tmp/connect.png" 로 수정
  String contentType = "image/png";
  int fileLength = 1116303;

      response.setHeader("Content-Disposition", "attachment; filename=\"" + fileName + "\";");
      response.setHeader("Content-Transfer-Encoding", "binary");
      response.setHeader("Content-Type", contentType);
      response.setHeader("Content-Length", "" + fileLength);
      response.setHeader("Pragma", "no-cache;");
      response.setHeader("Expires", "-1;");

      try(
              FileInputStream fis = new FileInputStream(saveFileName);
              OutputStream out = response.getOutputStream();
      ){
            int readCount = 0;
            byte[] buffer = new byte[1024];
          while((readCount = fis.read(buffer)) != -1){
              out.write(buffer,0,readCount);
          }
      }catch(Exception ex){
          throw new RuntimeException("file Save Error");
      }
}
```

-	입출력(IO) 메서드를 통해 file을 읽거나 쓴다.<br><br>

---

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16816/)
