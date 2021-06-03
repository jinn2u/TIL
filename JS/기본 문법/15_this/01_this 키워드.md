# this 키워드

- 동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다. 이때 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

- 객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.

```js
const circle = {
   radius: 5,
   getDiameter(){
      return 2* circle.radius
   }
}
console.log(circle.getDiameter())
```

getDiameter 메서드 내에서 메서드 자신이 속한 객체를 가리키는 식별자 circle을 참조하고 있다. 이 표현식이 평가되는 시점은 getDiameter가 호출되어 함수 몸체가 실행되는 시점이다.

- 위 예제의 객체 리터럴은 circle 변수에 할당되기 직전에 평가된다. 따라서 getDiameter메서드가 호출되는 시점에는 이미 객체 리터럴의 평가가 완료되어 객체가 생성되었고 circle 식별자에 생성된 객체가 할당된 이후다. 따라서 메서드 내부에서 circle 식별자를 참조할 수 있다.

### 하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지 않으며 바람직하지 않다.

```js
function Circle(radius){
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
   ???.radius = radius;
}
Circle.prototype.getDiameter = function(){
    // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
   return 2 * ???.radius
}
// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다.
const circle = new Circle(5)
```

- 생성자 함수에 의한 객체 생성 방식은 먼저 생성자 함수를 정의한 이후 new 연산자와 함께 생성자 함수를 호출해야 한다.
- 따라서 생성자 함수 정의 시점에서 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
- 이를 위해 this라는 특수한 식별자가 생겼다.
- this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.
- this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
- this가 가리키는 값, 즉 this 바인딩은 함수 호출방식에 따라 동적으로 결정된다.



```js
//위의 두 예제의 수정
const circle = {
   radius: 5,
   getDiameter(){
      return 2* this.radius
   }
}
console.log(circle.getDiameter())


function Circle(radius){
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
   thisradius = radius;
}
Circle.prototype.getDiameter = function(){
   return 2 * this.radius
}
const circle = new Circle(5)
```

