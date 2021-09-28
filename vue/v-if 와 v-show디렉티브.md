if문

아래와 같은 형태로 if문을 활용할 수 있다.

```vue
<h1 v-if="isTrue">Hello</h1>
<h2 v-else-if="0">My name is</h2>
<h2 v-else>jinn2u</h2>
```

### template 키워드

```vue
<h1 v-if="isTrue">Hello</h1>
<template v-else-if="0">
  <h2>My</h2>
  <h2>name</h2>
  <h2>is</h2>
</template>
<h2 v-else>jinn2u</h2>
```

template 키워드를 사용한다면 tempate은 생략되며 하위요소들만 돔에 렌더링이 되는것을 확인할 수 있다.

## v-show 디렉티브

v-show를 사용한다면 DOM tree에는 존재하지만 display:none으로 인해 렌더링은 되지 않는다.

하지만 v-if의 경우 아예 존재를 하지 않으며 개발자 도구를 확인하였을 때 주석으로 처리되어있는것을 확인할 수 있다.

### v-cloak 디렉티브

` 컴포넌트 인스턴스가 준비될 깨 까지 컴파일되지 않은 mustache 바인딩을 숨길수 있다.`

v-show는 아래와 같은 코드를 작성한다면 데이터가 들어오기까지 잠시의 시간동안 존재할 수 있다.

즉, {{msg}} 가 그대로 화면에 노출될 수 있다. 따라서 v-cloak을 통해 예방하도록 한다.

```vue
<style>
  [v-cloak] {
    display: none;
  }
</style>
<h1 v-show"isShow" v-clock>{{msg}}</h1>
```

### v-if와의 차이점

- v-if는 전환비용이 높지만 렌더링되지 않으므로 초기 렌더링 비용은 낮다.
- v-show는 전환비용은 낮지만 초기 렌더링 비용은 높다.
- 따라서 상황에 맞게 적절히 사용하자.

## v-if의 주의점

아래와같이 사용하지말자...! v-if 가 v-for보다 우선순위가 높다.

```vue
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```

따라서 아래와 같이 사용하도록 한다.

```vue
<template v-for="todo in todos">
  <li v-if="!todo.isComplete">
    {{ todo }}
  </li>
</template>
```
