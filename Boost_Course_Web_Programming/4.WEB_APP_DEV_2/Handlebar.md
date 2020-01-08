Handlebar
---------

---

Handlebar
---------

-	templating을 처리하는 라이브러리로서, tagged template literals와 pre-complie에 대해 더 공부해보자.<br><br>

기본적인 사용 방법
------------------

```javascript
<script type="myTemplate" id="listTemplate">
    <li>
     <div>게시자 : {{name}}</div>
     <div class="content">{{content}}</div>
     <div>좋아요 갯수 <span> {{like}} </span></div>
     <div class="comment">
       <div>{{comment}}</div>
     </div>
  </li>
</script>   

<script>
var data = {
    "id" : 88,
    "name" : "crong",
    "content" : "새로운글을 올렸어요",
    "like" : 5,
    "comment" : "댓글이다"
};

var template = document.querySelector("#listTemplate").innerText;
var bindTemplate = Handlebars.compile(template);  //bindTemplate은 메서드입니다.
bindTemplate(data);
</script>
```

<br>

배열이 포함된 데이터 처리
-------------------------

```javascript
<script type="myTemplate" id="listTemplate">
    <li>
        <div>게시자 : {{name}}</div>
        <div class="content">{{content}}</div>
        <div>좋아요 갯수 <span> {{like}} </span></div>
        <div class="comment">
        <h3>댓글목록</h3>
        {{#each comment}}
            <div>{{@index}}번째 댓글 : {{this}}</div>
        {{/each}}
        </div>
    </li>
</script>   

<script>
var data = {
    "id" : 88,
    "name" : "crong",
    "content" : "새로운글을 올렸어요",
    "like" : 5,
    "comment" : ["댓글이다", "멋진글이네요", "잘봤습니다"]
};
</script>
```

<br>

data 자체가 많아진 경우의 처리
------------------------------

```javascript
var innerHtml = "";

data.forEach(function (item, index) {
    innerHtml += bindTemplate(item);
});

var innerHtml = data.reduce(function(prve, next) {
    return prve + bindTemplate(next);
}, "");
```

<br>

조건 상황에 따른 처리
---------------------

```javascript
<script type="myTemplate" id="listTemplate">
    <li>
        <div>게시자 : {{name}}</div>
        <div class="content">{{content}}</div>
        <div>좋아요 갯수 <span> {{like}} </span></div>
        <div class="comment">
        <h3>댓글목록</h3>
        {{#if comment}}
            {{#each comment}}
                <div>{{@index}}번째 댓글 : {{this}}</div>
            {{/each}}
        {{else}}
            <div>댓글이 아직 없군요</div>
        {{/if}}
        </div>
    </li>
</script>
```

<br>

Helper function
---------------

```javascript
<script type="myTemplate" id="listTemplate">
    <li>
        <div>게시자 : {{name}}</div>
        <div class="content">{{content}}</div>

        {{#likes like}}
            {{like}}
        {{/likes}}

        <div class="comment">
        <h3>댓글목록</h3>
        {{#if comment}}
            {{#each comment}}
                <div>{{@index}}번째 댓글 : {{this}}</div>
            {{/each}}
        {{else}}
            <div>댓글이 아직 없군요</div>
        {{/if}}
        </div>
    </li>
</script>   

<script>
Handlebars.registerHelper("likes", function (like) {
   if (like > 4) {
       return "<span>축하해요 좋아요가 " + like + "개 이상입니다!</span>";
   } else if (like < 1) {
       return "아직 아무도 좋아하지 않아요..";
   } else {
       return like + "개의 좋아요가 있네요";
   }
});
</script>
```

<br>

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16784/)

---
