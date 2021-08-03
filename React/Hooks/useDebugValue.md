

```jsx
import useChangeAppState, { STATE_RUNNING, STATE_START } from "./useChangeAppState";
export default function App () {
  const [state, next] = useChangeAppState(true)
  const msg = 
  state === STATE_START
    ? '앱 시작'
    : state === STATE_RUNNING
    ? '앱 시작'
    : '앱 종료'
  return (
    <div>
      <p>{msg}</p>
      <button onClick={next}>next</button>
    </div>
  )
}
```

> ![image](https://user-images.githubusercontent.com/70435257/127888960-e2bd0d28-ca4b-4acd-80a8-c5bc77ffaeb4.png) <br />
> 리엑트 개발자 도구에서도 상수로 나오기 때문에 위와같이 에러가 있는 코드에서 디버깅하는것은 쉽지 않다.
>
> ```jsx
> export default function useChangeAppState(){
> const [state, setState] = useState(STATE_START)
> const next = () => setState(state === STATE_STOP ? STATE_START : state + 1)
> useDebugValue(
> state ===STATE_START 
>   ? 'start'
>   : state === STATE_RUNNING
>   ? 'running'
>   : 'stop'
> ) 
> return [state,next]
> }
> export const STATE_START = 0
> export const STATE_RUNNING = 1
> export const STATE_STOP = 2
> ```
>
> 이렇게 customHook 사용시 useDebugValue훅을 사용한다면 디버깅을 보다 쉽게 할 수 있다.<br />
> ![image](https://user-images.githubusercontent.com/70435257/127889681-7848c7f1-876e-401b-9810-be686d59519d.png)
