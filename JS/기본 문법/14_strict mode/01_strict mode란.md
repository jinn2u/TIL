```js
function foo(){
	x = 10
}
foo()
console.log(x) // 10

```

- foo 함수에서 선언하지 않은 변수 x에 값 10을 할당하였다. 이때 x 변수를 찾아야 x에 값을 할당할 수 있기 때문에 JS엔진은 x 변수가 어디에 선언되었는지 스코프 체인을 통해 검색하기 시작한다.

- 이때 JS엔진은 암묵적으로 전역 객체에 x프로퍼티를 동적 생성한다. (전역변수처럼 사용할수 있다.) 이러한 현상을 암묵적 전역이라 한다.
- 이러한 현상을 줄여야 예기치 않은 오류를 없앨수 있는데, strict mode를 통해서 JS문법을 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 JS엔진의 최적화 작업에 문제를 이르킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

## Strict mode의 적용

전역의 선두 또는 함수 몸체의 선두에 'use strict'를 추가한다.

```js
'use strict'
function foo(){
	x = 10 //ReferenceError
}
foo()
console.log(x)
```

## 주의점

- 스크립트단위로의 strict mode 사용

  - strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 수 있다.
  - 특히 써드파티 라이브러리를 사용하는 경우 라이브러리가 non-strict mode인 경우도 있기 때문에 전역에 strict mode를 적용하는것은 바람직하지 않다.
  - 이러한 경우 죽시 실행함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행함수의 선두에 strict mode를 적용한다.

- 함수 단위의 strict mode 사용

  - strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 문제가 된다.

  - ```js
    (function(){
       var let = 10;
       function foo(){
          'use strict';
          let = 20 //SyntaxError
       }
       foo()
    }())
    ```

    
