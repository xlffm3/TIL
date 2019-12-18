HTML Templating
---------------

---

### HTML Templating<br>

-	반복적인 HTML 부분을 template로 제작한 뒤, 서버에서 온 JSON 데이터를 결합하여 화면에 추가하는 작업이다.<br><br>

![image](https://user-images.githubusercontent.com/56240505/70503744-7236d900-1b67-11ea-8a58-7ff2f924e7a6.png)<br><br>

### 참고 코드<br>

```javascript
var data = {  title : "hello",
              content : "lorem dkfief",
              price : 2000
           };
var html = "<li><h4>{title}</h4><p>{content}</p><div>{price}</div></li>";

var resultHtml = html.replace("{title}", data.title)
                                    .replace("{content}", data.content)
                                    .replace("{price}", data.price)
```

<br>

### HTML Template의 보관<br>

```javascript
//HTML
<script id="template-list-item" type="text/template">
  <li>
      <h4>{title}</h4><p>{content}</p><div>{price}</div>
  </li>
</script>

//Js
var html = document.querySelector("template-list-item");
```

-	서버에서 file로 보관하고, Ajax로 요청해서 받아온다.<br><br>
-	HTML 코드 안에 숨겨둔다. <br><br>
-	예제 코드의 경우, HTML 중 script 태그는 type이 javascript가 아니면 렌더링을 하지 않고 무시하기 때문에 이를 이용한다.<br><br>

### 참고 코드<br>

```javascript
var data = [
        {title : "hello",content : "lorem dkfief",price : 2000},
        {title : "hello",content : "lorem dkfief",price : 2000}
];

//html 에 script에서 가져온 html template.
var html = document.querySelector("#template-list-item").innerHTML;

var resultHTML = "";

for(var i=0; i<data.length; i++) {
    resultHTML += html.replace("{title}", data[i].title)
                      .replace("{content}", data[i].content)
                      .replace("{price}", data[i].price);
}

document.querySelector(".content").innerHTML = resultHTML;
```

<br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/20732/)

---
