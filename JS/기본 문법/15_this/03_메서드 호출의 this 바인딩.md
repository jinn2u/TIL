## 메서드 호출

- 메서드를 호출할 때 이름 앞의 마침표 연산자 앞에 기술한 객체가 비인딩된다.

- 메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다는 것이다.

```js
const person = {
   name: 'jin',
   getName(){
      return this.name
   }
}
const otherPerson = {
   name: 'dong'
}
otherPerson.getName = person.getName
//otherPerson에서 getName메서드를 호출하였다.
console.log(otherPerson.getName()) // dong

const getName = person.getName
//일반함수로서 getName메서드를 호출하였다. 따라서 this.name은 global.name과 동일하다.
console.log(getName()) // undefined
```

