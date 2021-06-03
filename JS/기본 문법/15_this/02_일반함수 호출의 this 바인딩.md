- 함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위스코프를 결정한다.
- this바인딩은 함수 호출 시점에 결정된다.

## 일반 함수 호출

```js
function foo(){
   console.log("foo's this: ", this); //global
   function bar(){
      console.log("bar's this: ", this) // global
   }
   bar()
}
foo()
```

- 객체를 생성하지 않는 일반함수에서 this는 의미가 없다.

- 콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 this에도 전역 객체가 바인딩된다.
- 즉, 어떠한 함수라도 일반함수로 호출되면 this에 전역 객체가 바인딩된다.

```js
var value = 1;
const obj = {
   value: 100,
   foo(){
     //// 메소드이기 때문에 전역 객체가 바인딩되는것이 아니다.
      console.log("foo's this: ", this) // foo's this:  { value: 100, foo: [Function: foo] }
     
      setTimeout(function(){
        // 일반 함수안에서 호출되기 때문에 전역객체가 바인딩된다.
         console.log("callback's this: ", this) // global
         console.log("callback's this.vaue: ", this.value) // 1
      }, 100) 
   }
}
obj.foo()
```

### this 바인딩을 일치 시키는법

```js
var value = 1;
const obj = {
   value: 100,
   foo(){
     const that = this
      setTimeout(function(){
         console.log("callback's this.vaue: ", that.value) // 100
      }, 100) 
   }
}
obj.foo()
```

```js
var value = 1;
const obj = {
   value: 100,
   foo(){
      setTimeout(function(){
         console.log("callback's this.vaue: ", that.value) // 100
      }.bind(this), 100) 
   }
}
obj.foo()
```

```js
var value = 1;
const obj = {
   value: 100,
   foo(){
      setTimeout(()=>console.log(this.value), 100)
   }
}
obj.foo()
```

