this
===

this 에 대해서는 워낙 변화무쌍한 값이라 할말이 많습니다.

일반 함수에서의 this
---
```javascript
var data = 10;
function outer() {
    this.date = 20;
    data = 30;

    console.log(`1. data = ${data}`);
    console.log(`2. this.data = ${this.data}`);
    console.log(`3. window.data = ${window.data}`);
};

outer();
```

결과
---
![8](https://user-images.githubusercontent.com/32993709/50571593-c1ba4400-0df1-11e9-8928-03931f511d4f.PNG)

Why?
---
일반 함수 내부에서 this 는 전역객체인 window 를 저장합니다.

첫 줄에 변수로 선언한 data 는 window.data 와 동일합니다.

즉 함수 내부의 ```this.data === window.data``` 는 동일합니다.

그리고 함수 내부의 data 는 해당 변수를 찾을때까지 밖으로 나가기때문에 결국 전역변수 data 를 가리키고 값을 덮어쓰게 됩니다.

즉 일반 함수에는 this 이던 window 건 이미 선언된 변수건 모두 data 를 가르키게 됩니다.

다른 예제
---
```javascript  
var data = 10;

function outer() {
    this.date = 20;
    data = 30;

    console.log(`1. data = ${data}`);
    console.log(`2. this.data = ${this.data}`);
    console.log(`3. window.data = ${window.data}`);
}

console.log(`4. outer 함수 실행전 data = ${data}`);

outer();
console.log(`5. outer 함수 실행 후 data = ${data}`);
```

결과
---
![9](https://user-images.githubusercontent.com/32993709/50571645-24f8a600-0df3-11e9-9da4-63e1728fab25.PNG)

why?
---

함수 바깥에서 data를 찍어보았는데 함수의 실행전과 후가 다르고 4번으로 저장한 console이 제일 첫번째로 찍혀있다.

함수는 선언했을때는 아무 효력이 없고 실행문이 나왔을때 부터 적용되기 때문에 함수가 실행되기전엔 기존의 data 값이 찍히게 된다.

이후는 당연히 함수가 실행된 후기때문에 data에는 최종값인 30이 들어가있다.

---

중첩함수에서의 this
---
```javascript
var data = 10;
function outer() {
    // 중첩함수
    function inner() {
        this.data = 20;
        data = 30;

        console.log(`1. data1 = ${data}`);
        console.log(`2. this.data = ${this.data}`);
        console.log(`3. window.data = ${window.data}`);
    }
    inner();
}
outer();
```

중첩함수를 써도 동일하게 모두 30이 할당 됩니다.

함수가 내부에 하나 더 들어가있을뿐 동작하는데 별다른 영향이 있지는 않습니다.

---

이벤트 리스너에서의 this
---

```<button>Click</button>``` 을 하나 생성해 줍니다.

```javascript
var data = 10;
var btn = document.querySelector('button');
btn.addEventListener("click", function(){
    this.data = 20;
    data = 30;

    console.log(`1. data1 = ${data}`);
    console.log(`2. this.data = ${this.data}`);
    console.log(`3. window.data = ${window.data}`);
})
```

결과
---

![10](https://user-images.githubusercontent.com/32993709/50571906-7192b000-0df8-11e9-80da-06f71c17665c.PNG)

처음으로 변화가 생겼습니다.

일단 var data 와 click 함수 내부의 data 와 콘솔에 window.data 는 모두 같은 data 입니다.

그럼 this.data 에 생긴 변화를 보겠습니다.

why?
---

```javascript
var data = 10;
var btn = document.querySelector('button');
btn.addEventListener("click", function(){
    this.data = 20;
    data = 30;
    console.dir(this);
    console.log(`1. data1 = ${data}`);
    console.log(`2. this.data = ${this.data}`);
    console.log(`3. window.data = ${window.data}`);
})
```

결과
---
![11](https://user-images.githubusercontent.com/32993709/50571944-6c823080-0df9-11e9-88bf-2ce31dc6de6a.PNG)

```console.dir(this)``` 를 찍고 버튼을 클릭해보니 이벤트가 발생한 button 을 가르키고있습니다.

dir 을 사용하면 해당 객체의 모든 속성을 확인 할 수 있습니다.

여기서 빨간 표시부분을 보면 data 에는 20이 들어가 있습니다.

원래 this 는 일반함수에서 찍게되면 window 를 가르키게됩니다.

그치만 이벤트가 발생하게되면 발생한 이벤트를 this 를 가르키게되고 this.data 인 20의 값을 출력하게 됩니다.

---

메서드에서의 this
---

```javascript  
var data = 10;
function MyClass() {
    this.data = 0;
}

MyClass.prototype.method1 = function() {
    this.data = 20;
    data = 30;

    console.dir(this);
    console.log(`1. data1 = ${data}`);
    console.log(`2. this.data = ${this.data}`);
    console.log(`3. window.data = ${window.data}`);
}

// 인스턴스 생성
var my1 = new MyClass();
my1.method1();
```

결과
---
![12](https://user-images.githubusercontent.com/32993709/50572000-5f197600-0dfa-11e9-9790-fff0e8d0f3af.PNG)


why?
---

이벤트가 발생했을때와 동일한 결과가 나옵니다.

자바스크립트에서는 함수를 이용하여 클래스를 만들어 낼 수 있습니다.

클래스 함수 내부에서 지정한 this 는 일반함수가 아닌 인스턴스생성자로 쓰인 클래스 이기때문에 전역변수를 가르키지 않습니다.

이후 클래스에 prototype 체이닝으로 method1 이라는 메서드를 생성합니다.

그리고 새로운 인스턴스(클래스는 붕어빵 틀, 인스턴스는 붕어빵)를 생성하고 

그 내부에 메소드를 실행시킵니다.

결과를 보면 메소드 내부의 this 는 MyClass 객체를 가르키고있고 0을 처음에 입력하였지만
메소드 내부에서 this.data 는 20을 할당하고 있기때문에 값을 덮어쓰고

this 는 최종적으로 클래스 내부에있는 최종값 20을 가르키게 됩니다.

---

메서드 내부의 중첩함수 에서의 this
---
```javascript
var data = 10;

function MyClass() {
    console.log(this);
    this.data = 0;
}

MyClass.prototype.method1 = function() {
    
    function inner() {
        this.data = 20;
        data = 30;

        console.log(`내부 함수 에서의 ${this}`);
        console.log(`1. data1 = ${data}`);
        console.log(`2. this.data = ${this.data}`);
        console.log(`3. window.data = ${window.data}`);
    }
    return inner();
}

// 인스턴스 생성
var my1 = new MyClass();
my1.method1();
```

결과
---
![13](https://user-images.githubusercontent.com/32993709/50572355-3ac19780-0e02-11e9-93d9-059e79fd71f5.PNG)



다시 처음과 결과 값이 동일합니다.

MyClass 는 0이란 데이터를 갖고있고 중첩함수 내부의 this 는 window를 가르키고 있습니다.

Why?
---
자바스크립트의 [실행컨텍스트](https://poiemaweb.com/js-execution-context)와 연관성이 있습니다.

이 전의 예제에서는 함수의 실행컨텍스트가 class 객체였기 때문에 this 는 class 함수를 가르키고있었지만

이번 예제에서 내부함수는 메서드 내부에 정의되어있지만 실행컨텍스트가 MyClass 가 아니기때문에 내부의 this는 전역객체를 가르키게 됩니다.

따라서 method1 은 inner 함수가 반환한 값만 들고있을뿐 실행컨텍스트를 가르킬 this가 존재하지않습니다.

순서대로 설명하자면

1. my1 에 새로운 인스턴스 즉 MyClass 를 하나 새로만들어 저장합니다.
2. MyClass 에 지정한 메소드 method1() 을 호출합니다.
3. method1() 은 inner() 를 호출하고 함수내부에서 위에서 아래로 실행합니다.
4. this.data = 20 은 상위함수로 올라가서 data를 찾지만 값이 없기때문에 다시 전역으로 올라가 값을 찾고 전역변수 data 에 20을 덮어 씌움니다.
5. data 30 또한 전역변수를 찾아 30을 덮어씌우고 최종값 30만 반환합니다.
6. console 들은 전역변수의 값을 출력합니다.

inner() 함수는 호출한객체가 method1 이기 때문에 클래스함수에 속하지 않습니다.

그럼 이 this 를 유지해서 원래 목적인 실행컨텍스트의 this.data를 출력하려면 어떻게 해야할까요

중간에 this를 받아줄 변수를 생성해 주면됩니다.

```javascript
var data = 10;
function MyClass() {
    console.log(this);
    this.data = 0;
}

MyClass.prototype.method1 = function() {
    var that = this;
    function inner() {
        that.data = 20;
        data = 30;

        console.log(`내부 함수 에서의 ${that}`);
        console.log(`1. data1 = ${data}`);
        console.log(`2. this.data = ${that.data}`);
        console.log(`3. window.data = ${window.data}`);
    }
    return inner();
}

// 인스턴스 생성
var my1 = new MyClass();
my1.method1();
```

결과
---
![14](https://user-images.githubusercontent.com/32993709/50572440-401fe180-0e04-11e9-9bc2-3a41a26ce36e.PNG)


이전 예제와 동일하게 정상적으로 this 는 20을 받아오고있습니다.

메서드내부에 this 를 다른 변수에 바인딩 해 주었기 때문에 inner 함수의 실행컨텍스트가 MyClass 를 바라보게 되었습니다.  
MyClass 내부 data 값도 20을 나타내고 있습니다.

**위와 같은 방식을 Closer 라고도 합니다.**

---

정리
---
this 가 만들어 지는 경우 | this 값
---------|------------
일반함수에서 this | window 객체
중첩함수에서 this | window 객체
이벤트에서 this | 이벤트실행컨텍스트
메서드에서 this | 메서드실행컨텍스트
메서드 내부의 중첩함수에서 this | window 객체

---


주의!!
---


```javascript
var userName = "test";

function User(name) {
    this.userName = name;
}

// 호출
User("CodeReading");
console.log(`userName = ${userName}`)
```

위의 예제와

```javascript
var userName = "test";

function User(name) {
    this.userName = name;
}

// 호출
var user = new User("CodeReading");
console.log(`userName = ${userName}`)
```

이 예제를 비교해보고 어떤 값이 출력되는지 한번 생각해보자

> 답:  
CodeReading  
test

Why?
---

함수를 선언할때 첫글자를 대문자로 쓰면 class를 명시하는것이긴하지만 이건 하나의 약속일뿐 진짜 class 가 되는것은 아닙니다.

첫번째 예제와 같이 사용하게되면 그냥 일반 함수와 다를바 없고 

일반함수와 동일하게 this === window 가 됩니다

두번째 예제처럼 꼭 new User 로 생성해 줘야 클래스처럼 사용 할 수 있고 this 는 var user 를 가르키게 됩니다.

첫번째 예제처럼 출력하고싶으면

```console.log(`userName = ${user.userName}`)```

으로 똑같이 출력 할 수 있습니다.


어떻게 사용할까?
---
this 는 상황에 따라 워낙 다양하게 쓰일수있는 생각과 다르게 동작하는 부분이라 이것저것 직접 해보는게 가장 좋습니다.

그렇지만 몇가지 단순한 예제를 들어보겠습니다.

**예제1**

```javascript
function People(name, hobby, age, kg) {
    this.name = name,
    this.hobby = hobby,
    this.age = age,
    this.kg = kg,
    this.walking = function() {
        return kg - 1;
    }
}

var codeReading = new People("CodeReading", "coding", 32, 60);
console.log(codeReading.name);
console.log(codeReading.hobby);
console.log(codeReading.age);
console.log(codeReading.kg);
console.log(codeReading.walking());

var another = new People("Gildong", "Magic", 1000, 70);
console.log(another.name);
console.log(another.hobby);
console.log(another.age);
console.log(another.kg);
```

결과
---

![15](https://user-images.githubusercontent.com/32993709/50573234-7022b100-0e13-11e9-9e47-cec9631b8325.PNG)


위와같이 하나의 클래스를 생성하고 this 로 클래스의 인자를 받아서 

인스턴스 객체를 생성하여 얼마든지 또 다른 사람들을 생성해 낼 수 있습니다.

그냥 인스턴스에 인자값만 넘겨주면 됩니다.

**예제2**

```javascript
function Family(firstName) {
    this.firstName = firstName;
    var names = ['bill', 'team', 'json'];
    names.map(
        function (value, index) {
            console.log(`${value} ${this.firstName}`)
        }.bind(this)
    )
}

var code = new Family('code');   
```

결과
---
> bill code  
team code  
json code

위의 예제에서 map() 은 배열의 매소드로 전체 배열을 돌면서 function() 을 실행합니다.

value 는 배열의 각각의 값이 들어가고 index 는 배열의 index 값이 들어갑니다.

.bind(this) 는 this 에 강제로 바인딩하여 this 를 함수 밖에서도 사용 할 수 있도록 해주는 역할을 합니다.

최신 ES6 문법에서는 this 없이 사용할 수 있는 방법이 있습니다

Arrow 함수라고 불리우는 화살표 함수 입니다.

```javascript
function Family(firstName) {
    this.firstName = firstName;
    var names = ['bill', 'team', 'json'];
    names.map((value, index) => {
        console.log(`${value} ${this.firstName}`)
    })
}

var code = new Family('code');      
```

위와같이 사용하면 .bind 도 that 도 사용하지않고 더 깔끔하게 코드 작성이 가능합니다.

다만 일부 브라우저의 하위버전에서 작동하지 않기때문에 바벨같은 컴파일러로 ES5 문법으로 변환해서 업로드하는 방법을 사용해야 합니다.

ES6 문법에 대한 내용은 별도로 다루도록 하겠습니다.