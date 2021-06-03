# instance 연산자

- 객체 instanceof 생성자함수 형태로 사용한다.
- 우변의 생성자함수의 prototype에 바인딩된 객체가 죄변의 프로토타입 체인상에 존재하면 true를 반환한다.

```js
function Person(name){
	this.name = name
}
const me = new Person('jin')
console.log(me instanceof Person) // true
```



# 직접 상속

- Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성한다.

- 첫뻔째 매개변수에는 생성할 객체의 프로토타입으로 지정할 객체를 전달하며 두번째 매개변수에는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달한다. 두번째 인수는 옵션이다.

  ```js
  // 프로토타입이 null인 객체를 상성한다. 생성된 객체는 프로토타입 체인의 종점에 위치한다.
  // 종점에 위치하기 때문에 object.protype의 빌트인 메서드를 사용할 수 없다. 따라서 call을 통해 간접적으로 호츌해야 한다.
  let obj = Object.create(null)
  
  console.log(Object.getPrototypeOf(obj) == null); // true
  console.log(obj.toString()); // TypeError =>Object.prototype을 상속받지 못한다.
  console.log(obj.hasOwnProperty('x')); // error
  console.log(Object.hasOwnProperty.call(obj,'x')) // 로 간접 호출해야한다.
  
  obj = Object.create(Object.prototype) // obj = {}
  console.log(Object.getPrototypeOf(obj) === Object.prototype); // true
  
  obj = Object.create(Object.prototype, {
     x: {value:1, wratable: true, enumerable: true, configurable: true}
  }) // obj.x = 1과 동일하다.
  console.log(obj); // {x:1}
  
  const myProto = {x: 10}
  obj = Object.create(myProto)
  console.log(obj.x); // 10
  console.log(obj);  // {}      
  console.log(Object.getPrototypeOf(obj) === myProto); // true
  ```

- 이처럼 Object.create 메서드는 객체를 생성하면서 직접적으로 상속을 구현할 수 있다. 

- 장점

  - new 연산자 없이 객체를 생성할 수 있다.
  - 프로토타입을 지정하면서 객체를 생성할 수 있다.
  - 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.

- Object.create를 사용한 직접상속은 두번째 인자로 프로퍼티를 추가하는것이 불편하다.

- ES6부터는 객체 리터럴 내부에서 __proto__접근자 프로퍼티를 사용하여 직접상속을 구현할 수 있다.

  ```js
  const myProto = {x: 10}
  
  const obj = {
     y: 20,
     __proto__: myProto
  }
  console.log(obj.x,obj.y); // 10, 20
  console.log(obj); // {y: 20}
  console.log(Object.getPrototypeOf(obj) == myProto); // true
  ```
