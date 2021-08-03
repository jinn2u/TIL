useEffect 함수는 렌더링 결과가 돔에 반영된 후에 비동기로 호출된다.

useLayoutEffect는 useEffect와 거의 비슷하게 동작하지만 부수효과 함수를 동기로 호출한다. 즉, useLayoutEffect의 부수효과 함수는 렌더링 결과가 돔에 반영된 직후에 바로 호출된다. 때문에 연산을 많이한다면 브라우저가 먹통이될 수 있다.

따라서 특별한 이유가 없다면 useEffect를 사용하는것이 성능상 이점이 있다.

## useLayoutEffect

렌더링 직후에 돔 요소의 값을 읽어들이는 경우, 또는 조건에 따라서 컴포넌트를 다시 렌더링 하고 싶은경우 사용한다.

```jsx
export default function App () {
  const [width, setWidth] = useState(200)
  useEffect(() => {
    if (width > 500) {
      setWidth(500)
    }
  }, [width])
  return (
    <div>
      <div style={{width, height: 100, backgroundColor: 'green'}}>Test</div>
      <button onClick={() => {
        const value = Math.floor(Math.random()*499 + 1) 
        setWidth(value)
      }}>
        500 이하
      </button>
      <button onClick={() => {
        const value = Math.floor(Math.random()*500 + 501) 
        setWidth(value)
      }}>
        500 이상
      </button>
    </div>
  )
}
```

> 위의 경우 500이상 버튼을 눌렀을 때 깜빡거리는 현상이 발생하는데 이는 맨처음 500이상의 width로 렌더링이 된 이후 useEffect가 실행이 되면서 500으로 다시 렌더링하기 때문이다.  이러한 경우 useLayoutEffect를 통하여 해결할 수 있다.
>
> ### 해결 방법
>
> ```jsx
>  useLayoutEffect(() => {
>     console.log(boxRef.current.getBoundingClientRect().width)
>     if (width > 500) {
>       setWidth(500)
>     }
>   }, [width])
>   return (
>     <div>
>       <div ref = {boxRef}style={{width, height: 100, backgroundColor: 'green'}}>Test</div>
> ```
>
> 위의 코드로 수정을 하면 해결괸다.
>
> useLayoutEffect는 리엑트가 렌더링을 하고 실제 돔에 반영은 했지만 브라우저가 화면을 그리기 전에 이 코드가 실행이 된다.
>
> useRef를 통해 로그를 찍어보면 500이 넘는값이 출력되었다가 다시 500으로 변환되는것을 확인할 수 있다.

