## 중첩문의 지원

```scss
.container {
  display: flex;
  .item {
    background-color: red;
    > span {
      background-color: blue;
      &:hover {
        background-color: green;
      }
    }
  }
}
```

이러한 scss는 아래와 같이 변경될 수 있다.

```scss
.container {
  display: flex;
}
.container .item {
  background-color: red;
}
.container .item > span {
  background-color: blue;
}
.container .item > span:hover {
  background-color: green;
}
```
