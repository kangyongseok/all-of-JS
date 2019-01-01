Script
===

* script 요소는 HTML 내부에서 사용됩니다.
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>스크립트요소 확인</title>

        <script type="text/javascript"></script>

    </head>
    <body>
        
    </body>
    </html>
    ```

* script 는 html 내부에서 어느 위치에 들어가도 됩니다.
* 전통적으로는 head 태그 내부에서 css 를 불러오는 태그들과 함께 사용되었습니다.
* 그러나 이러한 방식의 문제점은 위에서 아래로 해석하는 html 과 js 의 특성으로 인해 js 의 모든 파일과 스크립트를 실행한 다음에 페이지를 불러와 웹 화면의 로딩이 느려지고 스크립트 또한 의도한대로 실행되어지지 않습니다.
* 따라서 js 파일을 불러와서 사용시 body 태그내부의 가장 최 하단에 위치시키는걸 권장합니다.
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>스크립트요소 확인</title>
    </head>
    <body>
        <h1>제목</h1>
        <div>컨텐츠</div>

        <script type="text/javascript" src="example.js"></script>
    </body>
    </html>
    ```

-------


비동기 스크립트
---
> async

```<script type="text/javascript" async src="example.js"></script>```

- 외부 스크립트 파일을 불러올때만 유요합니다.
- 브라우저에게 파일을 즉시 내려 받으라고 명령합니다.
- 스크립트가 마크업 순서대로 실행된다는 보장이 없습니다.
- async 속성은 사용할때 다른 스크립트와 의존성이 없어야합니다.
- 다운로드가 끝나면 바로 스크립트를 실행합니다.
- 파이어폭스 3.6, 사파리 5, 크롬 7 에서 비동기 스크립트 지원


스크립트 처리 지연
---
> defer

```<script type="text/javascript" defer src="example.js"></script>```

- 외부 스크립트 파일을 불러올때만 유요합니다.
- 브라우저에게 파일을 즉시 내려 받으라고 명령합니다.
- 해당 스트립트를 만나면 바로 코드를 내려받지만 HTML 파싱이 완료된후 실행합니다.
- 스크립트간에 순서가 보장됩니다.
- IE4, 파이어폭스 3.5, 사파리 5, 크롬 7 에서부터 지원


브라우저 이슈
---
구식 브라우저에서는 defer 만 지원하기도해서 두 속성을 모두 같이 적기도합니다.

```<script type="text/javascript" async defer src="example.js"></script>```

이렇게 했을경우 async를 먼저 따르고 지원하지않는다면 defer 특성으 따르게 됩니다.

---

type
---
최신 브라우저에서는 명시하지않아도 자동으로 javascript로 인식하지만 일부 옛날 브라우저에서는 명시해줘야 하는경우도 있어서 text/javascript 를 포함해 주는것을 권장합니다.

---

XHTML 호환 문제 해결
---
```html
<script type="text/javascript">
    //<![CDATA[
        function compare(a, b) {
            if(a < b) {
                console.log("b 는 a보다 큽니다.")
            }
        }
    //]]
</script>
```

- XHTML 에서는 '<' 를 태그의 시작으로 간주하기 때문에 오류가 발생 합니다.
- 위와같은 방법으로 처리하면 정상적으로 표현이 가능합니다.
- 인라인 스크립트 작성할때만 사용합니다.

---

noscript
---

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>스크립트요소 확인</title>
    <script type="text/javascript" src="example.js"></script>
    
</head>
<body>
    <noscript>
        <p>This page requires a JavaScript-enabled browser</p>
    </noscript>
</body>
</html>
```

- 해당 태그는 브라우저가 스크립트를 지원하지 않거나
- 브라우저의 스크립트 지원이 꺼져있을때만 나타나게 됩니다.

---

요약
---
- html 과 script 의 특성상 일반적으로 사용할 경우 body태그의 안쪽 가장 하단에 넣는것이 오류없이 실행이 잘 됩니다.
- 그치만 html 의 내용이 늘어날경우 가독성 부분에서 좋지 않기 때문에 head 태그내부에서 사용할 경우 ```defer, async``` 를 사용하거나 .js 파일 내부에서 ```window.onload = function() {//스크립트작성//}``` 을 사용하면 html 파싱이 끝난 후 스트립트를 실행하게 됩니다.
- html 내부에있는 모든 태그는 위에서부터 아래로 읽어내려갑니다.

**head 태그 내부에 스크립트 파일을 불러왔을때**

![1](https://user-images.githubusercontent.com/32993709/50570547-29ad6200-0dd3-11e9-85c6-4265d3b86532.PNG)


**js 를 body 태그 맨밑에 불러왔을때**

![2](https://user-images.githubusercontent.com/32993709/50570552-52cdf280-0dd3-11e9-823c-19ffe6512289.PNG)

**defer / async 옵션을 넣었을때**

![3](https://user-images.githubusercontent.com/32993709/50570555-6b3e0d00-0dd3-11e9-9d66-2cdf6b7e64ad.PNG)

**window.onload 를 사용했을때**

![4](https://user-images.githubusercontent.com/32993709/50570559-814bcd80-0dd3-11e9-83ca-060108dd6bf7.PNG)

---

그래서 어떻게 쓰나
---

- 의존성이 없고 html 랜더링이 끝나고 다른 js 파일보다 먼저 실행되어야 할경우 ```async```
- script 태그는 head 태그 내부에서 가장 마지막에 작성, 대신에 ``window.onload`` 나 jQuery 의 ```$(document).ready``` 로 브라우저 로딩이 끝난후 코드가 실행 되도록 작성
- 몇몇 웹 사이트를 돌며 head 내부 script 사용을 확인해보니 defer 는 거의 안쓰고 async 를 사용
- 다른 도메인에서 js 를 불러올경우 crossorigin 옵션을 작성하기도함

참고
---
- [꼼꼼히 살펴보는 SCRIPT 엘리먼트](https://taegon.kim/archives/6804)
- 프론트엔드 개발자를위한 자바스크립트 프로그래밍 (노랭이도서)