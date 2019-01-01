KARMA + Jasmine
===

- JavaScript 단위테스트 환경 구축
- KARMA : 테스트를 실행시킬 테스트 러너
- Jasmine : 테스트 코드를 수행할 테스팅 프레임워크

구축방법
---
```javascript
// 디렉터리 생성
# mkdir js-unit-test && cd js-unit-test

// pakage.json 생성
# npm init
test command에 'karma start' 입력

// 필요한 모듈 설치 개발자모드로
# npm install karma karma-chrome-launcher jasmine-core --save-dev

// karma cli 설치 테스트 실행환경 정의
# npm install -g karma-cli

// 카르마 설정파일 생성
# karma init
js 디렉토리의 .js 파일들을 테스트 하기 위해 경로설정
// js/*.js

// js-unit-test 폴더 하위에 js 폴더 생성 && 테스트 코드 작성
'use strict';

var sayHello = function() {
    return 'hello';
};

//test code
describe('sayHello.js', function() { // 단위테스트 명세 집합
    it('should returns string "hello"', function() { // 단위테스트 함수
        expect(sayHello()).toBe('hello'); // 매치함수
        expect(sayHello()).not.toBe('bye');
    });
});

// 테스트 실행
# npm run test
```

성공시
---
![5](https://user-images.githubusercontent.com/32993709/50571251-ba426d00-0de8-11e9-9909-7518b3a89810.PNG)

실패시
---
![6](https://user-images.githubusercontent.com/32993709/50571255-c9291f80-0de8-11e9-9e8d-af8f8e336dbf.PNG)


console
---
![7](https://user-images.githubusercontent.com/32993709/50571259-e231d080-0de8-11e9-91df-35c12df6cbb5.PNG)
