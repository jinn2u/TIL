## 생성자 함수 호출

- 생성자 함수 내부의 this에는 생성자 함수가 미래에 생성할 인스턴스가 바인딩된다.

```js
// 생성자 함수
const circle = {
   radius: 5,
   getDiameter(){
      return 2* this.radius
   }
}

const c1 = new Circle(5)
const c2 = new Circle(10)

console.log(c1.getDiameter()) // 10
console.log(c1.getDiameter()) // 20

const c3 = Circle(15)
//일반함수로 호출된 Circle은 반환문이 없으므로 undefined를 반환한다.
console.log(c3) // undefined
// 일반함수로 호출된 Circle내부의 this는 전역 객체를 가리킨다.
console.log(radius) // 15
```

