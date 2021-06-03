## arguments 프로퍼티



- 함수 객체의 arguments프로퍼티 값은 arguments 객체다. Arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이며 함수내부에서 지역변수처럼 사용된다. 즉 함수 외부에서 참조할 수 없다.

- Arguments 객체는 함수 내부에서 지역변수처럼 사용할 수 있다.

- arguments객체는 인수를 프로퍼티 값으로 소유하며 프로퍼티 키는 인수의순서를 나타낸다.

  ```js
  function multiply(x,y){
  	console.log(arguments);
  	return x*y;
  }
  console.log(multiply()) //[Arguments] {} /  NaN
  console.log(multiply(1)) // [Arguments] { '0': 1 } / NaN
  console.log(multiply(1,2)) // [Arguments] { '0': 1, '1': 2 } / 2
  console.log(multiply(1,2,3)) // [Arguments] { '0': 1, '1': 2, '2': 3 } / 2
  ```

  - JS에서는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지않는다. 
  - 하지만 모든 인수는 암묵적으로 arguments객체의 프로퍼티로 보관된다.



### arguments객체의 Symbol프로퍼티

```js
function multiply(x,y){
   // 이터레이터
   const iterator = arguments[Symbol.iterator]();
   console.log(iterator.next());
   console.log(iterator.next());
   console.log(iterator.next());
   console.log(iterator.next());
   return x* y;
}
multiply(1,2,3)
// { value: 1, done: false }
// { value: 2, done: false }
// { value: 3, done: false }
// { value: undefined, done: true }
```

### arguments를 활용한 가변인자함수 구현하기

```js
function sum(){
   let res = 0;
   for(let i = 0; i< arguments.length; i++){
      res += arguments[i];
   }
   return res;
}
console.log(sum()); // 0
console.log(sum(1,2)); // 3
console.log(sum(1,2,3)); // 6
```

- arguments 객체는 배열 형태로 인자 정보를 담고 있지만 실제 배열이 아닌 유사 배열 객체이다. 따라서 length 프로퍼티를 사용할 수 있다.

- ES6에서부터 arguments 객체는 유사 배열 객체이면서 동시에 이터러블이다. (ES5에서는 이터러블의 개념이 없었다.)

- 유사 배열 객체는 배열이 아니므로 배열 메서드를 사용할 경우 에러가 발생한다. 따라서 Function.prototype.call, Function.prototype.apply를 사용해 간접 호출해야 한다.

  ```js
  function sum(){
    // arguments 객체를 배열로 변환
     const array = Array.prototype.slice.call(arguments)
     return array.reduce(function(pre, cur){
        return pre+cur
     },0)
  }
  console.log((sum(1,2)));
  ```

- 이러한 번거로움을 해결하기 위해 ES6에서는 Rest 파라미터를 도입했다.

  ```js
  function sum(...args){
     return args.reduce((pre,cur)=> pre+ cur, 0)
  }
  console.log(sum(1,2));
  ```

  

