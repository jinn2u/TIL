# Ref 사용시 주의 사항

```jsx
import { useEffect, useRef } from "react";

function App() {
  const inputRef = useRef()
  useEffect(()=> {
    inputRef.current.focus()
  },[])
  return (
    <div>
      <input type="text" ref={inputRef} />
      <button>저장</button>
    </div>
  )
}
export default App
```

> focus는 렌더링 결과가 실제 돔에 반영된 후에 접근할 수 있기 때문에 부수효과 함수안에서 접근할 수 있다. 
>  클래스형 컴포넌트에서 current는 해당 클래스의 인스턴스를 가리킨다.
>  함수형 컴포넌트는 인스턴스로 만들어지지 않지만 useInperativehadle이라는 훅을 사용하면 함수형 컴포넌트 내부의 변수와 함수를 외부로 노출시킬 수 있다.

## Ref의 컴포넌트 내부 사용

```jsx
<Button ref={buttonRef} />
```

> 일관성을 위해 ref를 위와같이 작성하고 싶을 수 있다. 하지만 ref라는 이름으로 작성을 하면 리엑트가 내부적으로 처리하기 때문에 Button컴포넌트 내부에서 그 값을 사용할 수가 없다.
>
> 이러한 경우 forwardRef라는 함수를 사용할 수 있다.
>
> ```jsx
> const Button = React.forwardRef(function Button({ onClick }, ref) {
>   return (
>       <button onClick={onClick} ref={ref}> 저장</button>
>   )
> })
> ```
>
> 

```jsx
export default function App() {
  const [text, setText] = useState(true)
  const [showText, setShowText] = useState(true)
  return (
    <div>
      {showText && (
        <input  type="text"
          ref={ref => ref && setText(INITIAL_TEXT)}
          value={text}
          onChange={e => setText(e.target.value)} 
        />
      )}
      <button onClick={e => setShowText(!showText)}>보이기/ 가리기</button>
    </div>
  )
}
const INITIAL_TEXT = "안녕하세요"
```

> 위의 방식처럼 UseRef를 사용하지 않고 Ref 속성값에 함수를 입력할 수 있는데 이때 함수는 생성될때와 소멸될때 한번씩 실행이 된다.
>
> 생성될때는 해당하는 요소의 레퍼런스가 넘어오고 소멸될때는 NULl값이 넘어온다.
>
> 따라서 위의 코드는 생성될떄 초기값으로 INITIAL_TEXT를 설정해주는 것이다.
>
> 버튼을 누를 때 값을 토글 시키고 그 값에 따라서 input 요소를 보였다 사라졌다 하게 한다.
>
> 하지만 컴포넌트가 렌더링될때마다 새로운 ref 함수를 입력한다. 리엑트는 ref 속성값으로 새로운 함수가 들어오면 이전 함수에 null인수를 넣어서 호출한다. 그리고 새로운 함수에는 요소의 참조값을 넣어서 다시 호출한다.
>
> 때문에 이러한 문제를 해결하려면 함수를 고정할 필요가 있다. 이때 useCallback 훅을 사용할 수 있다.
>
>  useCallback의 메모이제이션 덕분에 한번 생성된 함수를 계속해서 재사용할 수 있다.
>
> ```jsx
> export default function App() {
>   const [text, setText] = useState(true)
>   const [showText, setShowText] = useState(true)
>   const setInitialText = useCallback(ref => ref && setText(INITIAL_TEXT),[])
>   return (
>     <div>
>       {showText && (
>         <input  type="text"
>           ref={setInitialText}
>           value={text}
>           onChange={e => setText(e.target.value)} 
>         />
>       )}
>       <button onClick={e => setShowText(!showText)}>보이기/ 가리기</button>
>     </div>
>   )
> }
> const INITIAL_TEXT = "안녕하세요"
> ```
>
> 

## 다수의 useRef 사용하기

```jsx
export default function App() {
  const boxListRef = useRef({})
  function onClick() {
    let maxRight = 0
    let maxId = ''
    for (const box of BOX_LIST){
      const ref = boxListRef.current[box.id]
      if(ref) {
        const rect = ref.getBoundingClientRect()
        if(maxRight < rect.right) {
          maxRight  = rect.right
          maxId = box.id
        }
      }
    }
    alert(`오른쪽 끝 요소는 ? ${maxId}입니다.`)
  }
  return (
    <div>
      <div
        style={{
          display: 'flex',
          flexWrap: 'wrap',
          width: '100vw',
          height: '100%'
        }}
      >
        {BOX_LIST.map(item => (
          <div
            key={item.id}
            ref={ref => (boxListRef.current[item.id] = ref)}
            style={{
              flex: '0 0 auto',
              width: item.width,
              height: 100,
              backgroundColor: 'yellow',
              border: 'solid 1px red'
            }}
          >{`box_${item.id}`}</div>
        ))}

      </div>
      <button onClick={onClick}>오른쪽 끝 요소는? </button>
    </div>
  )
}

const  BOX_LIST = [
  {id: 1, width: 70},
  {id: 2, width: 80},
  {id: 3, width: 800},
  {id: 4, width: 100},
  {id: 5, width: 300},
]
```

> 위와 같이 map을 순회하면서 값을 지정해줄 수 있다.

## current값 검사하기

```jsx
export default function App() {
  const inputRef = useRef()
  const [showText, setShowText] = useState(true)

  return (
    <div>
      {showText && <input type="text" ref={inputRef} />}
      <button onClick={() => setShowText(!showText)}>
      텍스트 보이기/ 가리기
      </button>
      <button onClick={() => inputRef.current.focus()}>텍스트로 이동</button>
    </div>
  )
}
```

> 조건부 렌더링 경우 current 속성이 존재하지 않을 수 도 있다. 위의 코드는 input 창이 없을떄 current값 검사를 하지 않으므로 오류가 발생한다.
>
> ```jsx
> <button onClick={() => inputRef.current && inputRef.current.focus()}>텍스트로 이동</button>
> ```
>
> 위의 코드 처럼 검사를 한다면 에러가 발생하지 않는다.





