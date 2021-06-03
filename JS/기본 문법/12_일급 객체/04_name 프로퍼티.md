## name 프로퍼티

- ES5에서는 비표준이었다가 ES6에서 표준으로 전환되었다. 따라서 동작을 달리한다.
- ES5에서는 name프로퍼티는 빈 문자열을 갖는다. ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.

```js
// 기명함수 표현식
var namedFunc = function foo(){};
console.log(namedFunc.name); //foo

// 익명함수 표현식
var asd = function(){}
console.log(asd.name); // asd (ES5에서는 빈 문자열을 반환한다.)

// 함수 선언식
function bar(){}
console.log(bar.name); // bar
```

- 함수를 호출할때는 함수 이름이 아닌 함수 객체를 가리키는 식별자로 호출한다.

