# ContextApi 렌더링 주의 사항



## 자식 컴포넌트의 영향줄이기

```jsx
import React, { createContext, useContext } from "react"

const UserContext= createContext({username: 'unknown', age: 0})

export default function App() {
  const [username, setUsername] = useState('')
  const [age, setAge] = useState(0)
  const [count, setCount] = useState(0)
  return (
    <div>
      <UserContext.Provider value={{username, age}}>
        <Profile />
        <button onClick={()=> setCount(count + 1)}>증가</button>
      </UserContext.Provider>
    </div>
  )
}

const Profile = React.memo(function() {
  console.log('Profile render')
  return (
    <div>
      <Greeting />
    </div>
  )
})
function Greeting() {
  console.log('Greeting render')
  const username = useContext(UserContext)
  return <p>{`${username}님 안녕하세요`}</p>
}
```

> 첫번째로 Greeting컴포넌트는 UserConetxt.Provider 컴포넌트를 통해 데이터를 받고 있다. 
>
> 원래의 React는 하위 컴포넌트에 데이터를 전달해주기 위해서  모든 최종 목적지까지의 모든 컴포넌트에 의미없는 데이터 전달용 코드를 작성해야 한다. 이떄 ContextApi를 사용하면 이러한 단점을 해결할 수 있다.
>
> Greeting컴포넌트는 Profile컴포넌트 아래에 있지만 Profile컴포넌트가 초기에 마운트된 이후에 렌더링 되지 않더라도 username값이 바뀌었을 때 Greeting 컴포넌트만 렌더링된다.
>
> 
>
> ## 문제점
>
> 위의 코드를 보면 App컴포넌트에서 count가 증가할때마다 렌더링이 다시 된다. 이때 username은 값이 변하지 않았지만,  provider로 전달해주기 때문에 Greeting도 렌더링 되는것을 알 수 있다.
>
> 이는 UserConext.Provider에서 렌더링될 때 마다 새로운 객체가 만들어지기 때문이다.
>
> ## 개선 사항
>
> ```jsx
> export default function App() {
> const [user, setUser] = useState({username: 'jin', age: 23})
> const [age, setAge] = useState(0)
> const [count, setCount] = useState(0)
> return (
>  <div>
>    <UserContext.Provider value={{user}}>
>      <Profile />
>      <button onClick={()=> setCount(count + 1)}>증가</button>
>    </UserContext.Provider>
>  </div>
> )
> }
> ```
>
> 위와 같이 useState에서 객체를 초기화해준다면, 리로드 되더라도 객체는 생성되지 않기 때문에 Greeting 컴포넌트에 영향이 가지 않는다.

## 하위 컴포넌트에서 데이터 변경하기

```jsx
export default function App() {
  const [user, setUser] = useState({username: 'jin', count: 0})
  return (
    <div>
      <SetUserContext.Provider value={setUser}>
        <UserContext.Provider value={user}>
          <Profile />
        </UserContext.Provider>
      </SetUserContext.Provider>
    </div>
  )
}
function Greeting() {
  const setUser = useContext(SetUserContext)
  const {username, count} = useContext(UserContext)
  return (
    <React.Fragment>
      <p>{`${username}님 안녕하세요`}</p>
      <p>{`인사횟수: ${count}`}</p>
      <button onClick={()=> setUser({username, count: count +1})}>
        인사하기
      </button>
    </React.Fragment>
  )
}
```

> 위의 SetUserContext 컴포넌트처럼 set함수를 직접 전달해주면 된다.
>
> 이때 하위 컴포넌트에서는 이를 활용하면 되는데 useState초기값이 객체형식이므로 set해줄 때도 마찬가지로 username또한 입력해주어야한다.
>
> 이렇게 여러개의 데이터를 객체 형식으로 관리할때는 useState보다는 useReducer를 사용하는것이 낫다.





Greeting 컴포넌트는 부모 컴포넌트의 Provider를 만나기 전까지는 createContext에서 설정한 초기값 unknown을 가지고 있는다.

부모의 Provider를 찾지 못하였을 경우 unknown값이 계속 할당되기 때문에 Provider를 설정한 위치가 중요하다. 실수할 경우를 대비하여 Provider를 루트에서 jsx 부분 전체를 감싸는 방식으로 많이 작성한다.(Provider 컴포넌트를 찾지 못할 일이 거의 없다.)



