## prototype 프로퍼티

- prototype프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor만이 소유하는 프로퍼티다.

```js
// 함수 객체는 prototype 프로퍼티를 소유한다.
console.log(function () {}.hasOwnProperty("prototype")); // true

// 일반 객체는 prototype프로퍼티를 소유하지 않는다.
console.log({}.hasOwnProperty("prototype")); // false
```

- (생성자)의 prototype프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될때 생성자 함수가 생성할 인스턴스의 프로토타입객체를 가리킨다.
- Person{prototype: Person.prototype}
<img width="800" src="https://user-images.githubusercontent.com/70435257/119533443-cd42a700-bdc0-11eb-9266-007dc81ec2d5.png">
