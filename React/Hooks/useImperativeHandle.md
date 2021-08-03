```jsx
function Profile(_, ref) {
  const [name, setName] = useState('mike')
  const [age, setAge] = useState(0)
  
  useImperativeHandle(ref, () => ({
    addAge: value => setAge(age + value),
    getNameLength: () => name.length
  }))  
  return (
    <div>
      <p>{`name is ${name}`}</p>
      <p>{`age is ${age}`}</p>
    </div>
  )
}
export default forwardRef(Profile)
```

>  클래스형 컴포넌트의 부모 컴포넌트는 ref 객체를 통해 자식컴포넌트의 메서드를 호출할 수 있다. 
>
> 이 방식은 자식 컴포넌트의 내부 구현에 대한 의존성이 생기므로 지양하는것이 좋다.
>
> 하지만 꼭 필요한 경우 사용한다면 함수형 컴포넌트에서도 멤버 변수나 멤버함수가 있는것처럼 만들 수 있다.
>
> useImperativeHandle의 두번째 매개변수의 반환값이 부모의 ref가 참조하는값이 된다.

```jsx
export default function App () {
  const profileRef = useRef()
  const onClick = () => {
    if(profileRef.current) {
      console.log(`current name length: `, profileRef.current.getNameLength())
      profileRef.current.addAge(5)
    }
  }
  return (
    <div>
      <Profile ref = {profileRef} />
      <button onClick={onClick}>add age 5</button>
    </div>
  )
}
```

> 부모 컴포넌트에서는 useRef를 사용하여 ref 객체를 Profile컴포넌트의 ref 속성값으로 입력하고 있다.
>
> profileRef.current는 자식컴포넌트의 useImperativeHandle의 두번째 매개변수가 반환값을 참조하게 된다.

