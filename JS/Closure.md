# Closure

## 

## 🌱MDN에서의 정의

- 클로저는 독립적인 (자유) 변수를 가리키는 함수이다. 또는, 클로저 안에 정의된 함수는 만들어진 환경을 ‘기억한다’.

🐤 예제 1  

```jsx
function getClosure() {
  var text = 'variable 1';
  return function() {
    return text;
  };
}

var closure = getClosure();
console.log(closure()); // 'variable 1
```

위의getClosure()에서 반환된 함수가 클로저인데, MDN에서 정의된 내용에서도 말했듯 환경을 기억하고 있는 것처럼 보인다. 

예제 1을 살펴보자. 

1. 익명함수는 text를 찾아 출력하는 함수로 정의하였다. 
2. 익명함수는 외부 스코프 참조(outer lexical environment)로 getClosure()를 저장하였다.
3. global 변수인 closure는 getClosure()를 데려온다.
4. global환경에서 closure를 호출한다.
5. 익명함수는 자신의 스코프에서 text를 찾지만 없다.
6. 따라서 외부 스코프인 getClosure에서 text를 찾는다.

```

익명함수는 자신이 생성된 렉시컬스코프를 벗어난 global에서 closure란 이름으로 호출되었고, 스코프 탐색은 현재 실행스택과 관련이 없는 getClosure를 거쳐갔다. 
closure를 익명함수로 초기화 할때는 이미 익명함수의 외부 스코프(outer lexical environment)를 getClosure로 정한다음이다. 
따라서 익명함수의 생성과 직접 관련이 없는 global환경에서 아무리 익명함수를 호출하더라도 여전히 getClosure()에서 text를 찾는 것이다. 
이러한 (익명)함수를 클로저라 한다.
```

>이러한 현상은 JS의 스코프는 소스코드가 작성된 바로 그 문맥에서 결정되기 때문이다.
추가적으로 getClosure()의 렉시컬 환경 인스턴스는 getClosure()가 실행종료된 이후에는 GC(Garbage Collection)이 회수를 해야하지만 closure가 계속 익명함수를 참조하고 있기 때문에 회수되지 않는다. 
따라서 클로저 사용이 종료되면 참조를 제거하는것이 좋다.

## 🌱클로저의 활용

🐤 예제 2

```jsx
var base = 'Hello, ';
function sayHelloTo(name) {
  var text = base + name;
  return function() {
    console.log(text);
  };
}

var hello1 = sayHelloTo('a');
var hello2 = sayHelloTo('b');
var hello3 = sayHelloTo('c');
hello1(); // 'Hello, a'
hello2(); // 'Hello, b'
hello3(); // 'Hello,'
```

출력된 결과를 보면 text 변수가 동적으로 변화하고 있는 것처럼 보인다. <br>
하지만 실제로는 text라는 변수 자체가 여러 번 생성된 것이다. 즉, hello1()과 hello2(), hello3()은 서로 다른 환경을 가지고 있다.<br><br>

일반적으로 JavaScript에서 객체지향 프로그래밍을 말한다면 Prototype을 통해 객체를 다루는 것을 말한다.
Prototype을 통한 객체를 만들 때의 주요한 문제 중 하나는 Private variables에 대한 접근 권한 문제이다. 예제 코드를 보자.
<br>
🐤 예제 3

```jsx
function Hello(name) {
  this._name = name;
}

Hello.prototype.say = function() {
  console.log('Hello, ' + this._name);
}

var hello1 = new Hello('a');
var hello2 = new Hello('b');
var hello3 = new Hello('c');

hello1.say(); // 'Hello, a'
hello2.say(); // 'Hello, b'
hello3.say(); // 'Hello, c'
hello1._name = 'anonymous';
hello1.say(); // 'Hello, anonymous'
```

```
예제3 에서 Hello()로 생성된 객체들은 모두 name이라는 변수를 가지게 된다. 
변수명 앞에 underscore()를 포함했기 때문에 일반적인 JavaScript 네이밍 컨벤션을 생각해 봤을때 
이 변수는 Private variable으로 쓰고싶다는 의도를 알 수 있다. 
하지만 실제로는 여전히 외부에서도 쉽게 접근가능한 변수일 뿐이다.
```

🐤 예제 4

```jsx
function hello(name) {
  var _name = name;
  return function() {
    console.log('Hello, ' + _name);
  };
}

var hello1 = hello('승민');
var hello2 = hello('현섭');
var hello3 = hello('유근');

hello1(); // 'Hello, 승민'
hello2(); // 'Hello, 현섭'
hello3(); // 'Hello, 유근'
```

예제4 처럼 클로저를 통하여 은닉화를 한다면 _name변수에 특별한 인터페이스 제공 없이는 접근이 불가능하다.
<br><br>
## 🌱setTimeout에서의 Closure

클로저는 보통 정보은닉을 구현하거나 함수 팩토리를 생성할 때 사용된다.

 🐤 예제 5.  *정수 값을 갖는 리스트를 반복문으로 접근하여 해당 요소마다 3초를 지연시키고 값을 출력하라*

```jsx
const arr = [10, 12, 15, 21];
  for (var i = 0; i < arr.length; i++) {
    setTimeout(function() {
      console.log('The index of this number is: ' + i);
    }, 3000);
  }
```

예제 5의 실행결과는 4가 네번 출력된다.<br>
문제의 원인은 setTimeout 함수가 인덱스 i 를 반복하는 스코프 밖의 스코프를 갖는 클로져를 생성하기 때문이다. 3 초가 지난 후에 클로저가 실행되고 i 값을 출력할 때 반복문의 종료 값인 4 를 출력한다.      
1. for문이 실행이 된다면 i가 1 씩 증가하며서 setTimeout을 호출한다.
2. i가 4가 된다면 for-loop를 빠져나온다.
3. for문은 실행이 끝났지만 내부 함수는 setTimeout함수에 의해 여전히 참조되고 있기 때문에 closure가 형성된다.
4. setTimeout의 대기시간이 지난 후 내부 함수가 호출이 되고 내부함수의 i는 closure영역의 i를 참조하는데 i는 이미 4가 되어 있다.

🐤 예제 6

```jsx
const arr = [10, 12, 15, 21];
  for (var i = 0; i < arr.length; i++) {
    setTimeout(function(i_local) {
      return function() {
        console.log('The index of this number is: ' + i_local);
      }
    }(i), 3000);
  }
```

예제 6처럼 즉시 실행함수에  i값을 인자로 전달해주어 원하는 결과가 출력된다.

🐤 예제 7

```jsx
const arr = [10, 12, 15, 21];
  for (let i = 0; i < arr.length; i++) {
    setTimeout(function() {
      console.log('The index of this number is: ' + i);
    }, 3000);
  }
```

예제 7의 경우는 ES6 의 let을 활용한다.
let은 함수가 호출 될 때 마다 인덱스 i 값이 바인딩 되기 때문에 내부함수가 호출될때마다 다른 i값이 바인딩된다.
