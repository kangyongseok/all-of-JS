TypeScript
===

TypeScript 는 최신 JavaScript 유형이고 상위집합이고 JS 에서 쓰던건 TS 에서 도 모두 그대로 사용 가능하다. 

TS 는 그대로 쓰이는 것이아닌 JS 로의 컴파일을 거쳐서 사용할 수 있다.

JS 는 동적으로 입력되기 때문에 런타임에 실제로 인스턴스화 될때까지 그 변수의 유형을 알지 못한다.

TS 에서는 이러한 타입을 명확하게 하여 변수의 유형을 알지 못하고 진행될 때보다 오류의 발생을 사전에 방지할 수있다.

정확히는 정적타입시스템을 도입한 자바스크립트라고 볼 수 있겠다.

- 정적타입언어 : 프로그램을 실행해보지 않고도 런타임 이전에 진행 (TypeScript)
- 동적타입언어 : 프로그램이 실제로 실행 될 때에 타입 분석을 진행 (JavaScript)

---

타입스크립트 기초 문법
---

**타입표기의 형태**

```javascript
const boy: boolean = true;
const age: number = 32;
const job: string = "programmer";
const description: string = `
Hello TypeScript!
I enjoy programming
`
const skill: Object = {
    javascript: true,
    ES6: true,
    JAVA: false
};
const nullValue: null = null;
const undefinedValue: undefined = undefined;
```

**특별한 타입**

```javascript
// 모든 타입과 호환 가능한 any
let sample: any = true;
    sample = 3;
    sample = "anytime";
    sample = {};
```

- 꼭 필요한 경우만 사용
- 일단 무시하고 넘어가고 이후에 정확히 적고 싶은 부분 또는 형태를 알수 없는 값등의 타입 표기에 유용

```javascript
// null 과 undefined 만 가지는 void
function empty(): void { }
```

- 아무런 값도 반환하지 않는 함수의 반환 타입을 표시


```javascript
// 아무런 값도 가지지 못하는 never
function whatIsThis(): never {
    throw new Error(`I'm a wicked function!`);
}
```

- never 타입은 절대 존재할 수 없는 값을 암시한다.
- null 이나 undefined 를 포함 어떤 값도 할당할 수 없다.
- 위의 예제는 항상 에러를 throw 하여 어떠한 값도 반환하지 않는다.

---

배열
---

```javascript
const numArray: number[] = [0, 1, 2, 3, 4, 5, 6];
const strArray: string[] = ["JavaScript", "Node.js", "MongoDB"];
// 또다른 방법
const numArray: Array<number> = [0, 1, 2, 3, 4, 5, 6];
const strArray: Array[string] = ["JavaScript", "Node.js", "MongoDB"];
```

튜플
---

```javascript
const nameAndAge: [string, number] = ["CodeReading", 32]
```

- 튜플은 명시된 타입의 갯수와 변수의 갯수가 정확히 맞아 떨어져야 한다. (2.7버전기준)

---

객체타입
---

```javascript
const user: {name: string; age:number} = {name: "CodeReding", age: 32};

// 선택속성
const nameWidthEmptyAge: {name: string; age?: number} = {
    name:"CodeReading"
};

// 읽기전용속성
const user: {
    readonly name: string;
    age: number;
} = {name:"CodeReading", age:32};

```

---

함수
---

```javascript
// 두 숫자의 합을 반환
function sum(a:number, b:number): number {
    return (a + b);
}

// 아무런 값도 반환하지 않는다면
function greetingsHello(name: string): void {
    console.log(`Hello, ${name}`);
}
```

- 함수 이름 옆에 괄호안에는 각 인자의 타입을 지정
- 괄호 밖의 ```:``` 다음에는 반환할 함수값의 타입을 지정

**기본 매개변수**

```javascript
function greetings(name: string = 'CodeReading'): void {
    console.log(`Hello, ${name}`);
}
greeting("JavaScript"); // Hello, JavaScript
greetung(); // Hello, CodeReading
```

**함수 값의 타입 표기**

```javascript
const anotherSum: (a: number, b: number) => number = sum;
const two: () => number = () => 2;
const sum: (a: number, b: number) => number = (a, b) => (a + b);
```

- 화살표 함수 처럼 사용 가능
- 인자의 타입을 지정 => 반환할 값의 타입 지정 => 반환할값


**TypeScript 의 This타입**

- typescript 의 interface 는 타입체크를 위해 사용되며 변수, 함수, 클래스에 사용
- 인터페이스에 선언된 프로퍼티 또는 메소드의 구현을 강제하여 일관성을 유지할 수 있도록 하는것
- 인터페이스는 프로퍼티와 메소드를 가질 수 있다는 점에서 클래스와 유사하지만 직접 인스턴스를 생성할 수 없고 모든 메소드는 추상 메소드

```javascript
interface HTMLElement {
    tagName: string;
}
interface Handler {
    (this: HTMLElement, event: Event, callback: () => void): void;
}
let cb: any;

const onClick: Handler = function(event, cb) {
    console.log(this.tagName);
    cb();
}
```

- this의 타입을 void 로 명시하면 함수 내부에서 this에 접근하는 일 자체를 막을수 있다.

```javascript   
interface NoThis {
    (this:void): void;
}

const noThis: NoThis = function() {
    console.log(this.a)
}
```
