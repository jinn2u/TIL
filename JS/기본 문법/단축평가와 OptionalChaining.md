## π±  λ¨μΆνκ°

```js
var done = true;
var message = ''

if(done) message = 'μλ£';

message = done && 'μλ£'
console.log(message) // μλ£

// --------------------
if(!done) message = 'μλ£'

message = done || 'μλ£'
console.log(done) // μλ£
```

## π±  μ΅μλ μ²΄μ΄λ

```js
var elem = null;

var value = elem ?. elem.value;
console.log(value); // null
```

- μ΅μλ μ²΄μ΄λ μ°μ°μ ?.λ μ’ν­ νΌμ°μ°μκ° falseλ‘ νκ°λλ falsy(false, undefined, null, 0, -0, nan, '')μ΄λΌλ null λλ undefinedκ° μλλ©΄ μ°ν­μ νλ‘νΌν° μ°Έμ‘°λ₯Ό μ΄μ΄κ°λ€.

<hr />

π±   λ¨μΆνκ°μ λ¨μ κ³Ό κ°μ 

```js
var foo = '' || 'default string'

console.log(foo); // default string
```

λ§μΌ fooκ° ''κ° λν μ ν¨νκ°μ΄λΌλ©΄, μκΈ°μΉ μμ λ°νμ νλ€. μ΄λ΄ κ²½μ° μ΅μλ μ²΄μ΄λμ μ΄μ©νλ€.

```jsx
var foo = '' ?? 'default string'
console.log(foo) // ""
```