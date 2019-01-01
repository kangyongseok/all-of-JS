null-and-undefined
===

null 과 undefined 의 차이
---
```javascript
let a
console.log(a) // undefined
console.log(typeof a) // undefined

let b = null
console.log(b) // null
console.log(typeof b) // object

console.log(null === undefined) // false
console.log(null == undefined) //true
```

- 둘다 값이 없음을 나타내긴 합니다
- ```undefined``` 는 명시하지 않으면 자동으로 할당된다.
- ```null``` 은 명시해야 한다. 주로 초깃값 설정시 사용한다.
- 이유는 해당 규칙을 지킬경우 ```undefined``` 가 나왔을경우 초기화가 되지않은것인지 확인이 가능
- ```undefined``` 와 ```null``` 은 '==' 일반 비교시엔 같지만
- 엄격 비교시 '===' 에는 서로 다르다고 나온다.
- '===' 는 타입까지 비교하기때문에 비교구문을 사용할땐 '===' 의 사용을 권장한다.

데이터 초기화
---
```javascript
// number 변수 초기화
var dataNum = 0;

// string 초기화
var dataString = "";

// Boolean 초기화
var dataBool = false;

// Object 초기화
var dataObj = null;

// Array 초기화
var dataArray = [];
```

이렇게 초기화한 방식을 보고 데이터의 타입을 알 수 있습니다.

상황에 맞는 변수초기화를 사용해주면되는데 

보시다시피 여기엔 undefined 는 없습니다.

undefined 는 변수선언만해도 할당되는 값이라 굳이 또 할당을 해줄필요도 없을 뿐더러 사용처는 디버깅할때 값의 초기화 확인 여부 정도 라고 할 수 있겠습니다.