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

플러그인 만들기
---

tab Menu 플러그인을 제작해 보겠습니다.

## HTML

```html
<script>
    $(document).ready(function() {
        $(".tab-area ul li").tabClick("#2196F3");
    })
</script>

<div class="wrap">
    <div class="tab-area">
        <ul>
            <li class="visible">TAB MENU 1</li>
            <li>TAB MENU 2</li>
            <li>TAB MENU 3</li>
            <li>TAB MENU 4</li>
        </ul>
        <div class="tab-underbar js-tab-underbar"></div>
    </div>
    <div class="contents">
        <div class="cont-box visible">contents box 1</div>
        <div class="cont-box">contents box 2</div>
        <div class="cont-box">contents box 3</div>
        <div class="cont-box">contents box 4</div>
    </div>
</div>
```

## CSS

```css
* {
    margin:0;
    padding:0;
}

html,
body {
    width:100%;
    height:100%;
    font-family:Roboto, sans-serif;
    background: #212121
}

.wrap {
    width:50%;
    margin:10% auto;
    box-shadow: 2px 2px 15px 5px #ffffff;
}

.tab-area,
.contents {
    background: #424242;
}

.tab-area ul {
    list-style:none;
    display:flex;
}


.tab-area ul li {
    color:white;
    cursor:pointer;
    padding:1rem 2rem;
}

.tab-underbar {
    width:155px;
}


.contents {
    border-top:1px solid white;
    height:200px;
    font-size:1.2rem;
    display: flex;
    flex-direction:row;
    flex-wrap: nowrap;
    
    
}

.cont-box {
    width:100%;
    color:white;
    margin:2rem;
    display: none;

}

.visible {
    display: block;
}

.b1 {
    flex-grow: 1;
}
```


## JS

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

---

CLASS 기반 플러그인 짜기
---

위와 같은 형태로 짜도 동작은 하지만 직접 만들어 사용하는 플러그인인 만큼 기능의 추가나 삭제 수정 또한 편해야 합니다.

그러나 위의 JS 코드는 나중에 제가 다시봐도 사실 수정을 어디서부터 어떻게 건드려야할지 잘 모를것같습니다.

나름 정리한다고 주석도 달고는 했지만 어디서부터 어디까지가 어떤 역할을 하는지에 대해서 한눈에 파악하기 어려운점도 있습니다.

그래서 기능도 추가하기 쉽고 가독성도 좋으면서 어느 부분이 어떤 역할과 기능을 맡아서 하는지 그리고 추가적으로 TAB MENU 를 적용해도 아무 문제 없이 사용 할 수 있도록 CLASS 기반 플러그인으로 수정하도록 하겠습니다.

JS

```javascript
// CLASS 기반 플러그인 만들기

(function($) {
    // 클래스 정의
    function TabMenu() {
        this.tabMenu = null;
        this.tabMenuLi = null;
        this.tabMenuBar = null;
        this.tabContents = null;
        this.tabContentsDiv = null;
    };

    // selector 지정
    TabMenu.prototype.init = function(selector, color) {
        this.tabMenu = $(selector);
        this.tabMenuLi = this.tabMenu.children();
        this.tabMenuFirstNode = this.tabMenuLi[0]
        this.tabMenuBar = this.tabMenu.siblings();
        this.tabContents = this.tabMenu.parent().siblings();
        this.tabContentsDiv = this.tabContents.children();
        this.tabContentsFirstDiv = this.tabContentsDiv[0];
        this.initEvent(color);
    };

    // 이벤트 실행 
    TabMenu.prototype.initEvent = function(color) {
        var that = this;
        this.initBase(color);
        _.map(this.tabMenuLi, function(tabMenuLi, index) {
            that.clickTab(that, tabMenuLi, color, index);
        })
    };

    // 기본 디자인 (컬러값 받아옴)
    TabMenu.prototype.initBase = function(color) {
        $(this.tabMenuFirstNode).addClass("on");
        _.map(this.tabMenuLi, function(tabMenuLi) {
            if($(tabMenuLi).hasClass("on")) {
                $(tabMenuLi).css("color", color)
            }
        });
        $(this.tabContentsFirstDiv).addClass("visible");
        $(this.tabMenuBar).css("border-top", "3px solid " + color)
    };

    // 클릭 이벤트 생성
    TabMenu.prototype.clickTab = function(that, tabMenuLi, color, index) {
        $(tabMenuLi).click(function() {
            that.tabEvent(tabMenuLi, color)
            that.tabContent(that, index);
            that.tabBarEvent(that, index);
        })
    };

    // 클릭시 발생할 탭 메뉴의 이벤트
    TabMenu.prototype.tabEvent = function(tabMenuLi, color) {
        $(tabMenuLi).addClass("on");
        $(tabMenuLi).siblings().removeClass("on");
        if($(tabMenuLi).hasClass("on")) {
            $(tabMenuLi).css("color", color);
            $(tabMenuLi).siblings().css("color", "white");
        }
    }

    // 클릭시 발생할 tabBar 의 이벤트
    TabMenu.prototype.tabBarEvent = function(that, index) {
        var barWidth = that.tabMenuBar.outerWidth();
        $(that.tabMenuBar).animate({marginLeft:barWidth * index}, 200)
    }

    // 클릭시 발생할 contents 의 상태
    TabMenu.prototype.tabContent = function(that, index) {
        $($(that.tabContentsDiv)[index]).addClass("visible");
        $($(that.tabContentsDiv)[index]).siblings().removeClass("visible");
    }

    // 인스턴스 생성
    $.fn.tabClick = function(color) {
        this.each(function() {
            var tabMenuInstence = new TabMenu();
            tabMenuInstence.init(this, color);
            console.log(tabMenuInstence);
        })
    }
})(jQuery)

```
기존보다 라인은 좀 길어졌습니다.  
그러나 기존보다 기능을 추가하거나 수정 삭제하기는 훨씬 쉬워졌습니다.  
underscore.js 를 같이 사용해서 편의성읖 높였습니다.  
jQuery 에 의존성은 가지지만 그만큼 브라우져호환에 있어서는 믿을만한 플러그인을 만들 수 있습니다.

사용법은 아래와 같습니다.

```javascript
$(".tab-area ul").tabClick("#2196F3");
```

색상도 인자값으로 받아 원하는 탭의 색상을 지정 할 수 있습니다.

