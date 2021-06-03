## 일반함수의 this

- strict mode에서 함수를 일반함수로서 호출하면 this에 undefined가 바인딩된다.

- 생성자 함수가 아닌 일반함수 내부에서는 this를 사용할 필요가 없기 때문이다. 

  ```js
  (function(){
     'use strict'
  
     function foo(){
        console.log(this) // undefined
     }
     foo()
     function Foo(){
        console.log(this) //Foo{}
     }
     new Foo()
  }())
  
  
  (function(){
     
     function foo(){
        console.log(this)
     }
     foo() //global을 바인딩한다.
     function Foo(){
        console.log(this)
     }
     new Foo() //Foo{}
  }())
  ```

## arguments 객체

```js
(function(a){
   'use strict'

   a = 2
   console.log(arguments) // [Arguments] { '0': 1 }
}(1))


(function(a){
   a = 2
   console.log(arguments) //[Arguments] { '0': 2 }
}(1))
```

