객체 지향 프로그래밍 (OOP)
===

## 특징

1. 추상화  
특정 클래스를 표현할때 예상되는 프로퍼티와 메서들르 정의하는 작업  
`클래스` `추상클래스` `인터페이스`

2. 캡슐화  
추상화 작업 내용중 외부에서 접근가능 여부에 대한 작업  
`접근지정자`

3. 상속  
상속은 특정클래스(부모클래스)의 속성과 메서드를 하위(자식) 클래스가 물려받는것  
`상속`

4. 다형성  
선언부분화 구현부분을 나눠 다양하게 처리  
`상속` `인터페이스` `추상클래스` `합성`

## 선언 부분과 구현 부분

1. 선언부분  
메서드의 이름이나 매개변수의 갯수 등 메서드정보가 선언부분이다.

2. 구현부분  
메서드의 기능을 직접 구현한 코드

| 부분    | 클래스 | 인터페이스 | 추상클래스 |
| ----- | --- | ----- | ----- |
| 선언 부분 | X   | O     | O     |
| 구현 부분 | O   | X     | O     |

---

추상화
---

객체지향 프로그래밍에서 추상화는 객체들의 공통적인 프로퍼티와 메서드를 뽑아내는 작업을 의미

```javascript 
// ES6 문법    
class Person {
    constructor (name, gender, hobby) {
        this.name = name;
        this.gender = gender;
        this.hobby = hobby;
    }

    talking() {
        console.log('말하다');
    };
    see() {
        console.log('보다');
    };
    eat() {
        console.log('먹다');
    };
    listen() {
        console.log('듣다');
    }
}
const chulsu = new Person("철수", "female", "baseball");
const code = new Person("Code", "female", "guitar");

console.log(chulsu); // Person { name: '철수', gender: 'female', hobby: 'baseball' }
chulsu.talking(); // 말하다
```

캡슐화
---

- 일반적으로 연관있는 변수와 함수를 클래스로 묶는 작업
- 캡슐화에는 은닉성이 포함
- 중요한 데이터나 기능을 외부에서 접근하지 못하도록

**접근 지정자**

| 접근지정자     | 객체 외부접근 | 객체 내부접근 | 자식객체 접근 |
| --------- | ------- | ------- | ------- |
| public    | O       | O       | O       |
| protected | X       | O       | O       |
| private   | X       | O       | X       |

1. public  
객체 외부와 객체 내부 그리고 자식 객체에서 모두 접근 가능한 프로퍼티와 메서드를 만든다.

2. protected  
객체 외부에서는 접근이 불가능하고 객체 내부와 자식 객체 에서만 접근 가능한 프로퍼티와 메서드를 만든다.

3. private  
오직 객체 자기 자신 에서만 접근 가능

- 자바스크립트에서는 기본적으로 캡슐화의 문법을 지원하지 않는다.

**JS에서의 방법**

```javascript
function MyClass() {
    // public
    this.프로퍼티이름 = 값;

    // private / protected
    this._프로퍼티이름 = 값;
}

// public 메서드
MyClass.prototype.메서드이름 = function() {}

// private / protected
MyClass.prototype._메서드이름 = function() {}
```

- `private / protected` 에 해당하는 메서드와 프로퍼티명에 `_` 를 붙임으로써 암묵적인 약속으로 흉내내어 사용한다.
- 그러나 암묵적인 약속일뿐 문법적으로는 접근이 가능하기 때문에 주의를 기울여 사용할 필요가 있다.


상속
---

- 클래스 상속을 활용하면 기존의 코드 변경없이 기능을 추가하거나 수정이 가능하다

**부모클래스**
```javascript
// 부모 클래스
function MyParent() {
    this.property1 = 10;
}

MyParent.prototype.method1 = function() {
    alert(this.property1);
}

```

**자식클래스**
```javascript
// 자식클래스
function MyChild() {}

// 부모 클래스를 상속받는 문법
MyChild.prototype = new MyParent();
```

**사용**

```javascript
// 인스턴스 생성
var child1 = new MyChild();

// 부모기능에 존재하는 매서드 호출
child1.method();
```

- `MyChild` 에는 `method1()` 이 없지만 부모클래스를 상속받았기 때문에 코드복사 없이 재사용이 가능하다.

- 객체지향 프로그래밍 네가지 특징중 유일하게 클래스 상속은 지원
- 프로토 타입을 통해 상속 구현


오버라이드
---

자식클래스에서 부모클래스의 기능을 재정의할때 사용

- 부모클래스의 기능을 사용하지않고 자식클래스에서 구현한 기능을 사용하고 싶은경우
- 부모클래스의 기능을 자식클래스에서 확장하고 싶은경우

**기존에 부모의 method 를 그대로 받아 사용하는경우**

```javascript
function MyParent() {
    this.property1 = "data1";
    console.log("MyParent()")
}
MyParent.prototype.method1 = function() {
    console.log("property1 = " + this.property1);
}

function MyChild() {
    console.log("MyChild()");
}
MyChild.prototype = new MyParent();
MyChild.prototype.constructor = MyChild;

var child1 = new MyChild();
child1.method1();
```

**부모의 method가 아닌 자식클래스에서 재정의한 method 로 실행**

```javascript
function MyParent() {
    this.property1 = "data1";
    console.log("MyParent()")
}
MyParent.prototype.method1 = function() {
    console.log("property1 = " + this.property1);
}

function MyChild() {
    console.log("MyChild()");
}
MyChild.prototype = new MyParent();
MyChild.prototype.constructor = MyChild;


// 오버라이드
MyChild.prototype.method1 = function() {
    console.log(`프로퍼티 1은 ${this.property1} 입니다.`)
}

var child1 = new MyChild();
child1.method1();
```

**부모 클래스의 기능을 자식 클래스에서 확장**

```javascript
function MyParent() {
    this.property1 = "data1";
    console.log("MyParent()")
}

MyParent.prototype.info = function() {
    console.log(`property ${this.property1}`);
}

function MyChild() {
    console.log("MyChild()");
    this.property2 = "data2";
}

MyChild.prototype = new MyParent();
MyChild.prototype.constuctor = MyChild;

// 기능확장
MyChild.prototype.info = function() {
    MyParent.prototype.info.call(this);
    console.log(`property2 = ${this.property2}`);
}

var child1 = new MyChild();
child1.info();
```