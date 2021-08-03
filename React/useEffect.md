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