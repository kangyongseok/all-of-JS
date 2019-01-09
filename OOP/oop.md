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

부분 | 클래스 | 인터페이스 | 추상클래스  
--- | --- | --- | ---
선언 부분 | X | O | O
구현 부분 | O | X | O

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