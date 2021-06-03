## 암묵적 전역

선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.

```js
(function(){
   'use strict'
   x = 1;
   console.log(x) //Reference Error: x is not defined
}())
```



## 변수, 함수, 매개변수의 삭제

delete 연산자로 변수, 함수, 매개변수를 삭제하면 syntaxError가 발생한다.

strict mode가 아니면 지워지지도 않고 에러도 발생하지 않는다.

```js
(function(){
   'use strict'

   var x = 1;
   delete x // SyntaxError

   function foo(a){
      delete a // SyntaxError
   }
   delete foo // SyntaxError
}())
```



## 매개변수 이름의 중복

```js
(function(){
   'use strict'
   
   // SyntaxError
   function foo(x,x){
      return x + x
   }
   console.log(foo(1,2))
}())



(function(){
   function foo(x,x){
      return x + x
   }
   console.log(foo(1,2)) // 4 뒤에 나온 매개변수로 초기화된다.
}())

```

## with문의 사용

```js
(function(){
   'use strict'

   a = {
      b: {
          c:{
              x: 5,
              y: 5,
              z: 5
          }
      }
  }
  // syntaxError
  with (a.b.c){
      x = 10;
      z = 12;
  }   
   
  console.log(a.b.c.x);   
  console.log(a.b.c.y);   
  console.log(a.b.c.z);   

}())


(function(){
   a = {
      b: {
          c:{
              x: 5,
              y: 5,
              z: 5
          }
      }
  }
   
  with (a.b.c){
      x = 10;
      z = 12;
  }   
   
  console.log(a.b.c.x);   // 10
  console.log(a.b.c.y);   // 5
  console.log(a.b.c.z);   // 12
}())
```

