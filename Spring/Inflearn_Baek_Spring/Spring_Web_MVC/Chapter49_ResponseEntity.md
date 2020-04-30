ResponseEntity
--------------

> FileController.java

```java
@Autowired
ResourceLoader resourceLoader;

@GetMapping("/file/{fileName}")
public ResponseEntity<Resource> fileDownload(@PathVariable String fileName) throws IOException {
    Resource resource = resourceLoader.getResource("classpath:" + fileName);
    File file = resource.getFile();
    Tika tika = new Tika();
    String type = tika.detect(file);
    return ResponseEntity.ok()
            .header(HttpHeaders.CONTENT_DISPOSITION, "attachement; filename=\"" +
            resource.getFilename() + "\"")
            .header(HttpHeaders.CONTENT_TYPE, type)
            .header(HttpHeaders.CONTENT_LENGTH, String.valueOf(file.length()))
            .body(resource);
}
```

-	응답 상태 코드와 응답 헤더 및 응답 본문 등을 설정할 수 있는 리턴 타입이다.<br><br>
-	Content-Disposition : 사용자가 해당 파일을 받을 때 사용할 파일 이름이다.<br><br>
-	Content-Type : 파일의 미디어 타입을 의미한다.<br><br>
-	Content-Length : 파일의 용량을 의미한다.<br><br>
-	ResponseBody를 붙여도 상관없으며, 붙이지 않아도 ResponseEntity가 응답임을 의미한다.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn).<br><br>
