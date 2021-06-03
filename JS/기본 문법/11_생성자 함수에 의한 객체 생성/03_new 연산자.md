## New 연산자

- 일반 함수와 생성자 함수의 특별한 차이는 없지만, new 연산자와 함께 함수를 호출하면 해당함수는 생성자 함수로 동작한다.
- 즉 [[Call]]이 호출되는것이 아니라 [[Construct]]가 호출된다.

```js
function add(x, y) {
  return x + y;
}
// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
let inst = new add();
// 함수가 객체를 반환하지 않으므로 반환문이 무시되고 빈객체가 return된다.
console.log(inst); // add {}

function createUser(name, role) {
  return { name, role };
}

inst = new createUser("Lee", "admin");
console.log(inst); // { name: 'Lee', role: 'admin' }
```

### new.target

```js
function Circle(radius) {
  // new와 함께 호출되지 않으면 new.target은 undefined이다.
  if (!new.target) {
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle = Circle(5);
console.log(circle.getDiameter());
console.log(circle); //Circle { radius: 5, getDiameter: [Function (anonymous)] }
```

- 생성자 함수가 new 연산자 없이 호출되는 것을 방지한다. 메타 프로퍼티라고 부른다.
- IE에서는 지원하지 않는다.

## instanceof

```js
function Circle(radius) {
  // 이 함수가 new연산자와 함께 호출되지 않았다면 이 시점의 this는 전역 객체 global을 가리킨다.
  // 따라서 if문에서 new를 생성자함수를 호출한다.
  if (!(this instanceof Circle)) {
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle = Circle(5);
console.log(circle.getDiameter());
console.log(circle); //Circle { radius: 5, getDiameter: [Function (anonymous)] }
```
