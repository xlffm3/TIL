MultipartFile
-------------

> FileController.java

```java
@Controller
public class FileController {

    @PostMapping("/file")
    public String fileUpload(@RequestParam("file") MultipartFile file, RedirectAttributes attributes) {
        //save
        String message = file.getOriginalFilename() + "is successfully uploaded.";
        attributes.addFlashAttribute("message", message);
        return "redirect:/file";
    }

    @GetMapping("/file")
    public String fileUploadForm(@ModelAttribute("message") String message, Model model) {
        model.addAttribute("message", message);
        return "/file/index";
    }
}
```

<br>

> index.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div th:if="${message}">
    <h2 th:text="${message}"/>
</div>
<form method="POST" enctype="multipart/form-data" action="#" th:action="@{/file}">
    File: <input type="file" name="file"/>
    <input type="submit" value="Upload"/>
</form>
</body>
</html>
```

<br>

> SampleControllerTest.java

```java
@Test
public void test() throws Exception {
    MockMultipartFile file = new MockMultipartFile("file",
            "test.txt",
            "text/plain",
            "hello file".getBytes());
    this.mockMvc.perform(multipart("/file")
    .file(file))
            .andDo(print())
            .andExpect(status().is3xxRedirection());
}
```

-	파일 업로드시 사용하는 메소드 아규먼트이다.<br><br>
-	MultipartResolver Bean이 설정 되어 있어야 사용할 수 있으나, Spring Boot는 자동 설정을 지원한다.<br><br>
-	POST multipart/form-data 요청에 들어있는 파일을 참조할 수 있다.<br><br>
-	List<MultipartFile> 아규먼트로 여러 파일을 참조할 수도 있다.<br><br>
-	관련 설정은 MultipartAutoConfiguration 및 MultipartProperties 등이 있다.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn).<br><br>
