# 스크린 리더

### Accessible name

스크린리더가 요소를 포커스 했을 때 읽는 값을 말한다.

1. author: 특별한 속성을 사용해서 정하는값
    
     - aria-label, aria-labelledby, alt
    
2. contents: 요소의 텍스트 값을 말한다.

우선 순위는 author > contents이다.

## Role

role은 스크린 리더가 요소를 어떤 방식으로 다룰지 결정하는 속성이다.

role마다 기대되는 속성이 있다.

`role=button`의 경우요소의 Accessible name을 읽은 뒤 “버튼"을 붙여 읽는다.

자식요소의 accessible Name을 모아서  content로 사용한다.

시멘틱 태그들은 암시적 role을 가지고 있다.

## children Presentational

```tsx
<div role="button>
	<span>1</span>
	<span>2</span>
	<span>3</span>
</div>
// 1 2 3 버튼을 읽는다.
```

div의 children은 1 2 3 이다. 

또한 div와 button은 children presentational특징을 가지고 있는데, 이는 자식 요소의 accessible name을 모아 요소의 content로 사용한다.

따라서 1 2 3 버튼을 읽게 된다.

## 이미지 태그와 alt

alt는 img의 author를 정하는 속성이다.

따라서 alt가 없다면 스크린리더가 src를 읽기 때문에 반드시 alt를 입력해야 한다.

## role=”checkbox”

role=’checkbox’

스크린리더가 체크박스라고 읽는다.

aria-checked=”true | false”와 함께 사용하면, 선택됨/ 선택 해제됨 도 읽어준다.

children presentational이다.

## role=”dialog”

사용자가 상호작용할 수 있는 대화상자이다.

aria-modal: 스크린리더가 dialog 밖의 요소에 포커스할 수 없게 만드는 속성이다.

스크린리더가 dialog만 포커스하게 되므로 사용자가 dialog를 인지할 수 있음

스크린리더가 구현하지 않은 경우가 있어 직접 구현이 필요하다. 따라서 modal dialog가 열렸을 때 dialog밖을 focus할 수 없도록 해야한다.

aria-hidden이 true일 경우 스크린리더가 해당 요소와 자식을 읽지 않는다.

```tsx
<div>
      <h1></h1>
      <h2></h2>
  </div>
  <div role="dialog">
    <button className="sr-only">닫기</button>
  </div>

// 모달을 제외한 부분에 aria-hidden 속성 추가하기

function openModal(dialogContainerElement) {
    [...document.body.children].forEach(element => {
      if(dialogContainerElement !== element) {
        element.setAttribute('aria-hidden', 'true')
      }
    })
  }
```

## role = “alert”

요소가 DOM Tree에 추가되거나 

요소의 자식에 변경사항이 생겼을 때

스크린리더가 해당 요소를 읽는다.