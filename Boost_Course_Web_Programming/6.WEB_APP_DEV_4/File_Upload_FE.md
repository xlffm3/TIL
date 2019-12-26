File Upload : Front-End
-----------------------

---

### File Upload<br>

```html
<div class="formWrap">
    <form action="/join" method="post" id="myform" enctype="multipart/form-data">
        <div class="inputWrap">
            <div class="email">
                <span> Email </span> <input type="text" name="email"><br/>
            </div>
            <div class="password">
                <span> Password </span> <input type="password" name="password"><br/>
            </div>
        <input type="file" name="reviewImg" id="uploader" accept="image/*">
        </div>
        <input class="sendbtn" type="submit">
    </form>
</div>
```

-	type을 file로 선언하고, 클라이언트/서버 간의 보낼 데이터의 name을 설정한다.<br><br>
-	accept는 허용 가능한 file type을 결정짓는다.<br><br>
-	이와 관련해 유용한 속성은 ***caniuse.com*** 등 사이트를 참조하고, 브라우저 지원이 제한적인 상태이므로 참고해서 사용해야 한다.<br><br>
-	id는 클라이언트 상에서 DOM 제어 용도로 사용한다.<br><br>
-	일반적인 form 데이터를 전송 시에 HTTP Header에는 기본적으로, 'application/x-www-form-urlencoded' 라는 정보로 노출된다.<br><br>
-	그러나 file의 경우, form tag의 속성으로 enctype를 multipart/form-data로 지정해야 한다.<br><br>

### DevTools : Network<br>

```javascript
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7rKYKhIWaTrjvGi1

------WebKitFormBoundaryiUIOhJXAwxorM25j
Content-Disposition: form-data; name="email"

werwerw@sefdsf.com
------WebKitFormBoundaryiUIOhJXAwxorM25j
Content-Disposition: form-data; name="password"

werwerwerwerwer
------WebKitFormBoundaryiUIOhJXAwxorM25j
Content-Disposition: form-data; name="reviewImg"; filename="A_Pen_by_younjisu.png"
Content-Type: image/png


------WebKitFormBoundaryiUIOhJXAwxorM25j--
```

-	WebKitFormBoundaryiUIOhJXAwxorM25j 라는 어떤 구분정보를 기준으로 데이터가 노출되고 있다.<br><br>
-	파일을 보낼 때는, 보통 다른 데이터와 별도로 먼저 보내는 경우도 많다.<br><br>
-	Ajax로 구현할 때, jQuery와 ***[FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects#Sending_files_using_a_FormData_object)*** 속성을 이용하면 좀 더 쉽게 구현이 가능하다.<br><br>

### 확장자 유효성 검사<br>

> accept 속성 사용<br>

```html
<input type="file" name="reviewImg" id="reviewImageFileOpenInput" accept="image/png, image/jpeg">
```

-	file 선택창에서 브라우저가 지정된 확장자 파일만 선택하도록 돕지만, 브라우저 지원 상황이 깔끔한 상태가 아니다.<br><br>

> change event 사용<br>

```javascript
const elImage = document.querySelector("#reviewImageFileOpenInput");
elImage.addEventListener("change", (evt) => {
  const image = evt.target.files[0]; //멀티파일 가능성
  if(!validImageType(image)) {
    console.warn("invalide image file type");
    return;
  }
})

function valideImageType(image) {
    const result = ([ 'image/jpeg',
                      'image/png',
                      'image/jpg' ].indexOf(image.type) > -1);
    return result;
}
```

-	file을 업로드 하면, change 이벤트를 통해서 이를 감지하고 file 객체를 얻을 수 있다.<br><br>

### 썸네일 노출<br>

```javascript
const elImage = document.querySelector("#reviewImageFileOpenInput");
elImage.addEventListener("change", (evt) => {
    const image = evt.target.files[0];
    if(!valideImageType(image)) {
        console.warn("invalide image file type");
        return;
    }
    //이렇게 넣으면 이미지 정보가 화면에 노출됩니다.
    const elImage = document.querySelector(".thumb_img");
    elImage.src = window.URL.createObjectURL(image);
})
```

-	Ajax로 img 파일을 서버로 전달한 뒤, img url 등 응답 정보를 받아 화면에 노출 시키는 것이 일반적인 과정이다.<br><br>
-	file 객체를 넣으면 file에 접근할 수 있는 url을 제공해주는 `createObjectURL()` 메서드로 쉽게 구현이 가능하다.<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16811/)

---
