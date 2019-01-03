make-jquery-plugin
===

jQuery 기본 동작
---
```javascript
$("div").css("border", "2px solid red");
```

위의 예제는 모든 div 를 선택해서 css 로 테두리에 빨간선을 추가하는 예제입니다.

우선 저걸 javascript 로 짜보겠습니다.

```javascript
var element = document.querySelectorAll("div");
[...element].map((el) => {
    el.style.border = "2px solid red"
})
```

위의 예제는 [undescore.js](https://underscorejs.org/) 를 사용하면 더 간결하게 쓸 수 있습니다.

```javascript
var element = document.querySelectorAll("div");
_.map(element, (el) => {el.style.border = "2px solid red"})
```

사실 이렇게만 봐서는 큰 차이점은 없지만 우선 먼저 사용한것은 map() 메소드는 배열만 사용할 수 있습니다. querySelectorAll 은 전체 태그를 가져오는데 이게 배열처럼 보이지만 배열이 아니기 때문에 위와같은 [...element] 처리를 해주어야 사용이 가능해서 좀 지저분해 보입니다.

반면에 _.map 은 어떤 객체가와도 배열이든 아니던 상관없이 적용이 가능합니다.

어쨋든 jQuery 보다는 라인수도 많아졌고 코드도 복잡해 보입니다.  
제이쿼리를 사용하면 기본적으로 정의된것 이외에 자신만의 플러그인을 만들어 사용 할 수 있습니다.

위의 예제로 플러그인을 만들어 보겠습니다.

```javascript
$.fn.borderRed = function() {
    this.css('border', '2px solid red');
}

$("div").borderRed();
```

이러한 방식으로 플러그인을 제작할 수 있는데

분석해보자면

```$.fn 은 jQuery.prototype 과 동일합니다. ```

javascript 에서 class 를 만들어 사용할때와 비슷한데

```$("div")``` 에 의해서 인스턴스가 생성됩니다.

따라서 메소드안의 this 는 생성된 인스턴스를 가르키게 됩니다.

---

class 기반 플러그인 만들기
---

tab Menu 플러그인을 제작해 보겠습니다.

```javascript

(function($) {
    $.fn.tabClick = function(color) {
        //  탭 메뉴의 컬러 색상을 받습니다.
        $(this[0]).css("color", color)
        // 지정된 노드 각각에 적용합니다.
        this.each(function(index) {
            var $target = $(this);
            var tabUnderbar = $target.parent().siblings();
            // tabmenu 의 언더바의 색상을 받습니다.
            tabUnderbar.css("border-bottom","3px solid " + color)
            // 대상 노드를 클릭했을때 발생할 내용입니다.
            $target.click(function() {
                var barWidth = $target.outerWidth();
                var tabMenuIndex = index;
                var $contents = $target.parent().parent().siblings().children();

                tabUnderbar.animate({marginLeft:barWidth * index}, 200);
                $target.css("color", color);
                $target.siblings().css("color", "white");

                // 컨텐츠의 visible none 실행부분입니다.
                _.map($contents[tabMenuIndex].classList, function(list) {
                    if(list !== "visible") {
                        $($contents[tabMenuIndex]).addClass("visible");
                        $($contents[tabMenuIndex]).siblings().removeClass("visible");
                    } 
                })
            })
        })
        return this;
    }
})(jQuery)



```


