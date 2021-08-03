```jsx
function Profile( { userId }){
  const [user, setUser] = useState()
  useEffect(() => {
    fetchUser(userId).then(data => setUser(data))
  })
}
```

>    useEffect의 부수효과 함수는 Profile 컴포넌트가 렌더링될 때마다 노툴되기 때문서버의 api를 호출하는 코드가 항상 실행된다. 
>
> 따라서 의존성 배열에 빈 배열을 입력함으로써 마운트된 후 한번만 호출되도록 할 수 있다.
>
> 하지만 빈 배열을 입력한다면 userId가 변경되어도 api를 호출하지 않기 때문에  의존성배열에 userId를 넣는다.
>
> 부수효과 함수 안에서 사용된 상태값이나 속성값 또는 그 두가지 값으로 게산된 어떤 값이 있을 때 그 값을 사용했다면 항상 그 값은 의존성 배열에 나열해줘야한다.
>
> userId가 변경되지 않는다는것을 확신한다면 별도의 훅으로 관리해주는것이 좋다.
>
> ```jsx
> function Profile( { userId }){
>   const [user, setUser] = useState()
>   useOnMounted(() => fetchUser(userId).then(data => setUser(data)))
> }
> ```
>
> ```jsx
> function useOnMounted(effect) {
> 	useEffect(effect, [])
> }
> ```
>
> 

부수효과 함수를 async await 함수로 만들 수 없다. 부수효과 함수의 반환값은 항상 함수 타입이어야 하기 때문이다. async await 함수는 Promies 객체를 반환하기 때문에 부수효과 함수가 될 수 없다. 따라서 useEffect 함수에서 async await 를 사용하고자 한다면 별도의 함수를 만들어주아야 한다.

```jsx
async funcdtion fetchAndSetUser(userId, needDetail){
	const data = await fetchUser(userId, needDetail)
	setUser(data)
}
function Profile(){
  const [user, setUser] = useState()
  useEffect(() => {
    fetchAndSetUser(false)
  },[fetchAndSetUser])
}
```

>  하지만 위와 같이 한다면, async 함수는 profile 컴포넌트가 렌더링될 때마다 생성이된다.따라서 의존성 배열의 내용은 항상 변하기 때문에 useEffect 함수 또한 렌더링될 때마다 실행이 된다. 이럴때 useCallback 훅을 사용할 수 있다.
>
> ```jsx
> const fetchAndSetUser = useCallback(
> async function (needDetail) {
> 	const data = await fetchUser(userId, needDetail)
> 	setUser(data)
> },[userId])
> ```
>
> useCallback을 통해서 userId가 변경될 때마다 함수가 새로 생성될 수 있도록 한다.



## 의존성배열에 신경을 쓰자

가능하다면 의존성 배열을 사용하지 않는것이 좋다. 의존성 배열을 관리하는데 생각보다 많은시간과 노력이 들어가기 때문이다.

특히 속성값으로 전달되는 함수를 의존성 배열에 넣는 순간 그 함수는 부모 컴포넌트에서 useCallback등을 사용해서 자주 변경되지 않도록 신경써서 관리를 해야한다. 

따라서 의존성 배열을 입력하지 않는 대신 부수효과 함수 내에서 실행시점을 조절할 수 있다.

부수효과 함수 내에서 처리하면 부수효과 함수 안에서 사용하는 모든 변수는 최신화된 값을 참조함으로 안심할 수 있다.

=> useCallback과 같은 훅을 사용하지 않아도 된다.

### 조건분기를 통해 실행시점 조절하기

```jsx
useEffect(()=> {
	if(!user || user.id !== userId) {
	fetchAndSeetUser(false)
	}
})
```



### 함수를 통해 값에 의존하지않기

```jsx
useEffect(() => {
	function onClick() {
		setCount(count + 1)
	}
	window.addEventListener('click', onClick)
	return () => window.removEventListener('click', onClick)
},[count])
```

>  위는 count 값을 1씩 증가시키기 위해 count를 의존성 배열에 추가했다.
>
> ```jsx
> useEffect(() => {
> 	function onClick() {
> 		setCount(prev => prev + 1)
> 	}
>   window.addEventListener('click', onClick)
> 	return () => window.removEventListener('click', onClick)
> })
> ```
>
> 위처럼 함수를 사용한다면 함수의 매개변수로 이전 상태값이 들어오기 때문에 의존성배열을 추가하지 않아도 된다.



## 여러 값을 참조하는 경우

```jsx
function Timer({ initialTotalSeconds }) {
  const [hour, setHour] = useState(Math.floor(initialTotalSeconds / 3600 ))
  const [minute, setMinute]= useState(
    Math.floor(initialTotalSeconds % 3600 / 60)
  )
  const [second, setSecond] = useState(initialTotalSeconds % 60)
  useEffect(() => {
    const id = setInterval(() => {
       
      }
    }, 1000)
    return () => clearInterval(id)
  },[hour, minute, second])
}
```

>  이러한 경우 useReducer를 사용하는것이 좋다. dispatch 함수는 변하지 않는 값이므로  useEffect에서 활용하기 적합하다.
>
> ```jsx
> function Timer({ initialTotalSeconds }) {
>   const [state, dispatch] = useReducer(reducer, {
>     hour: Math.floor(initialTotalSeconds / 3600 ),
>     minute: Math.floor(initialTotalSeconds % 3600 / 60)
>   })
>   const { hour, minute, second } = state
>   useEffect(() => {
>     const id = setInterval(dispatch, 1000)
>     return () => clearInterval(id)
>   })
>   function reducer(state) {
>     const { hour, minute, second } = state
>     if(second) {
>       return { ...state, second: second - 1}
>     } else if (minute) {
>       return { ...state, minute: minute -1, second: 59 }
>     } else if (hour) {
>       return { hour: hour -1, minute: 59, second: 59 }
>     } else {
>       return state
>     }
>   }
> }
> ```

## 변경이 애매한 경우

```jsx
function MyComponent ({ onClick }) {
  useEffect(() => {
    window.addEventListener('click', () => {
      onClick()
    })
  }, [onClick])
}
```

> 위와 같이 속성값으로 전달되는 함수는 자주 변경되는 경우가 많다. 내용은 그대로인데 렌더링할때마다 변경되는 경우가 많다. 때문에 부수효과 함수가 불필요하게 자주 호출될 수 있다.
>
> 마땅히 해결할 방법이 없다면, useRef 훅이 해결책이 될 수 있다.
>
> ### useRef로 해결하기 
>
> ```jsx
> function MyComponent ({ onClick }) {
>   const onClickRef = useRef()
>   useEffect(() => {
>     onClickRef.current = onClick
>   })
>   useEffect(() => {
>     window.addEventListener('click', () => {
>       onClickRef.current()
>     })
>   },[])
> }
> ```
>
> 부수효과 함수에서 사용된 ref객체는 의존성 배열에 추가할 필요가 없다.
>
> 이렇게 useEffect안에서 ref 객체를 수정하는 이유는 추후에 도입될 concurrent mode 때문이다.
>
> concurrent mode로 동작할 때는 컴포넌트 함수가 실행됐다고 하더라도 중간에 렌더링이 취소될 수 있다.
>
> 렌더링은 취소됐는데 Ref객체에는 잘못된 값이 저장될 수 있으므로 ref 객체는 컴포넌트 함수에서 직접 수정하면 안된다.
