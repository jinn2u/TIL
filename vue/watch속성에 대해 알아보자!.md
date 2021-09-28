# watch 속성에 대해 알아보자!

반응형 데이터에 관해서 watch속성을 통해 관리를 할 수 있다.

이떄 메서드명은 data에 정의한 것과 같은 이름이어야 한다.

```
data() {
	return {
		address: '경기도'
	}
},
watch: {
	address(newAddress, oldAddress) {
		console.log('watch: ', newAddress, oldAddress)
	},
},
```

1. 위와 같이 data의 이름과 동일한 이름으로 watch 안에 작성을한다.

2. 첫번째 인자로 새로운값이 들어오고, 두번째 인자로 기존의 값이 들어온다.

3. 기존의값이 필요없다면

   ```js
   address() {
     console.log('watch: ', this.address)
   },
   ```

   와 같은 형태로 새로운 값에 직접적으로 접근할 수도 있다.

## 문제점

위의 watch는 원시데이터는 잘 감시할 수 있지만, 객체데이터일 경우에는 데이터가 변하였는지 확인할 수 없다는 문제점이 있다.

## 해결하기

```js
watch: {
  family: {
    handler() {
      console.log(this.family)
    },
      deep: true,
        immediate: true
  },
},
```

이렇게 일반함수에서 객체로 변경을 한 다음에 handler를 통해서 변경된 사실을 확인할 수 있다.

또한 `deep` option을 통해서 원시값이 아닌 값까지 변경된 사실을 파악할 수 있다.

또한 watch는 맨 처음 DOM이 렌더링 될때는 실행되지 않고, 그 이후 데이터가 동적으로 변할 때 실행이되는데, 초기에 DOM이 렌더링될 때도 감시하고 싶다면 `immediate` 속성을 통해 확인할 수 있다.

### 전체 코드

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <script src="https://unpkg.com/vue@next"></script>

    <title>Document</title>
  </head>

  <body>
    <style>
      .title {
        font-size: 60px;
        color: blue;
      }

      .active {
        color: red;
      }

      .title--small {
        font-size: 40px;
      }

      .color--orange {
        color: orange;
      }
    </style>
    <div id="app">
      <button @click="toggle">Toggle</button>
      <ul>
        <li v-for="person in family">{{person.name}}</li>
      </ul>
    </div>
    <script>
      const App = {
        data() {
          return {
            family: [
              { id: 1, name: "roy" },
              { id: 2, name: "james" },
              { id: 3, name: "mark" },
            ],
          };
        },
        watch: {
          family: {
            handler() {
              console.log(this.family);
            },
            deep: true,
            immediate: true,
          },
        },
        methods: {
          toggle() {
            this.family.forEach((person) => {
              person.name = person.name.toUpperCase();
            });
          },
        },
      };
      const vm = Vue.createApp(App).mount("#app");
    </script>
  </body>
</html>
```
