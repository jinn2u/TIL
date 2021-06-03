## call과 apply

apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것이다. 

apply와 call 메서드는 함수를 호출하면서 첫번쨰 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.

```js
function getThisBinding(){
   console.log(arguments)
   return this 
}

const thisArg = {a: 1}

// apply가 실행한 getThisBinding()에서의 this는 thisargs이다.
// arguments:  [Arguments] { '0': 1, '1': 2, '2': 3 }
// this: { a: 1 }
console.log(getThisBinding.apply(thisArg,[1,2,3]))
// 결과는 apply와 동일하다. 단지, 인수를 쉼표로 구분하여 받는다.
console.log(getThisBinding.call(thisArg, 1, 2, 3))
```



apply와 call메서드의 대표적인 용도는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용할 수 없는 경우이다.

```js
function convertArgsToArray(){
   console.log(arguments)   // [Arguments] { '0': 1, '1': 2, '2': 3 }
  // slic를 인수없이 호출하면 배열의 복사본을 생성한다.
   const arr = Array.prototype.slice.call(arguments)
   console.log(arr) 
   return arr
}

convertArgsToArray(1,2,3) // [ 1, 2, 3 ]
```



## bind

```js
function getThisBindind(){
   return this
}

const thisArgs = {a:1}
// bind 메서드는 함수에 this로 사용할 객체를 전달한다.
// 바인드 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야한다.
console.log(getThisBindind.bind(thisArgs)())
```

