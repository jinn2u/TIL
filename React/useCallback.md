### UseCallBack

```jsx
export default function App() {
  const [name, setName] = useState('')
  const [age, setAge] = useState(0)
  const [v1, setV1] = useState(0)
  return (
    <div>
      <p>{`name is ${name}`}</p>
      <p>{`age is ${age}`}</p>
      <UserEdit
        onSave={()=> saveToServer(name, age)}
        setName={setName}
        setAge={setAge}
      />
      <p>{`v1: ${v1}`}</p>
      <button onClick={() => setV1(Math.random())}>v1 수정</button>
    </div>
  )
}
const UserEdit = React.memo(function ({ onSave, setName, setAge }){
  console.log(`userEdit render`)
  return null;
})
function saveToServer(name, age){}
```

> 컴포넌트가 렌더링 될때 마다 새로운 함수가 생성되서 입력이 된다. 이 컴포넌트가 렌더링될때마다 매번 속성값이 변경되기 때문에 자식 컴포넌트인 UserEdit 컴포넌트는 새로운 속성값을 입력받게 된다.(불필요한 렌더링 발생)
>
> ```jsx
> const onSave = useCallback(()=> saveToServer(name, age),[name, age])
> ```
>
> 위의 코드처럼 useCallback을 사용한다면 의존성 배열의 값이 바뀔때만 렌더링이 일어난다.