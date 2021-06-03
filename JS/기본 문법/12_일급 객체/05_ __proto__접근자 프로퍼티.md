## __proto__접근자 프로퍼티

- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다.
- [[Prototype]]내부 슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.
- __proto__ 프로퍼티는 [[Prototype]]내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티이다.

```js
const obj = {a:1}
// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__proto__ === Object.prototype); // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
console.log(obj.hasOwnProperty('a')); // true
// hasOwnproperty 메서드는 Object.prototype의 메서드다.
console.log(obj.hasOwnProperty('__proto__')); //false
```

