## 🌱  단축평가

```js
var done = true;
var message = ''

if(done) message = '완료';

message = done && '완료'
console.log(message) // 완료

// --------------------
if(!done) message = '완료'

message = done || '완료'
console.log(done) // 완료
```

## 🌱  옵셔널 체이닝

```js
var elem = null;

var value = elem ?. elem.value;
console.log(value); // null
```

- 옵셔널 체이닝 연산자 ?.는 좌항 피연산자가 false로 평가되는 falsy(false, undefined, null, 0, -0, nan, '')이라도 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.

<hr />

🌱   단축평가의 단점과 개선

```js
var foo = '' || 'default string'

console.log(foo); // default string
```

만일 foo가 ''값 또한 유효한값이라면, 예기치 않은 반환을 한다. 이럴 경우 옵셔널 체이닝을 이용한다.

```jsx
var foo = '' ?? 'default string'
console.log(foo) // ""
```