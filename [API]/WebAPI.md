Web API
-------

---

<br>

### Web API<br><br>

> -	URL은 정보의 자원을 표현해야 하며, 이를 HTTP Method로 표현한다.<br><br>
> -	HTTP Method<br>
>
> ```
> POST : 해당 URL를 요청하면 리소스를 생성한다.
> GET : 해당 리소스를 조회하고 해당 도큐먼트에 대한 자세한 정보를 가져온다.
> PUT : 해당 리소스를 수정한다.
> DELETE : 리소스를 삭제한다.
> ```

<br><br>

### 세부 규칙<br><br>

> -	`GET /members` , `DELETE/members/1` 처럼 HTTP Method로만 자원에 대한 행위를 표현한다.<br><br>
> -	나쁜 예 : `GET /members/delete/1` <br><br>
> -	URL 마지막 문자로 슬래시 구분자를 포함하지 않는다.<br><br>
> -	하이픈은 가독성을 높일 때 사용하며, 언더바는 사용하지 않고, 소문자로 URL 경로를 표기한다.<br><br>
> -	URL 문법 형식은 URL 스키마와 호스트를 제외하고는 대소문자를 식별하며, 파일 확장자는 URL에 포함하지 않는다.<br><br>
> -	Accept Header를 사용한다.<br><br>
> -	200번대 상태 코드는 성공이며, 300, 400, 500 번대는 오류이니 reference를 참조하라.

<br><br>

[Reference]

-	[REST API 제대로 알고 사용하기 : TOAST Meetup](https://meetup.toast.com/posts/92)<br>
-	[edwith](https://www.edwith.org/boostcourse-web/lecture/20654/)

---
