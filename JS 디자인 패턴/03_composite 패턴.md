## 1. Todo List 만들기

```js
const list1 = new TaskList('비사이드')
// 이름을 크게 짓고 리팩토링하면서 범위를 좁혀 나간다.
list1.add("지라 설치")
list1.add("지라 클라우드 접속")

const list2 = new TaskList("s75")
list2.add("2강 답안 작성")
list2.add("3강 교안 작성")

console.log(list1.byTitle())
console.log(list2.byDate())
```



## 2.

```js
const Task = class{
  constructor(title, date){
    if(!title) throw "invalid title"
    this._tilte = title
    this._date = date
    this._isComplete = false
  }
  isComplete(){return this._isComplete}
  toggle(){this._isComplete = !this._isComplete}
  sortTitle(task){
    return this._tilte > task._title
  }
  sortDate(task){
    return this._date > task._date
  }
}
const taskSort = {
  title: (a,b) => a.sortTitle(b),
  date: ( a,b) => a.sortDate(b)
}
```

>리스트는 task를 담고있기 때문에 list부터 파악하기 어렵다.
>의존성이 없는 작은 애들부터 파악하는것이쉽다.
>toggle의 경우 캡슐화를 통해 내부 사정을 숨긴다.(무슨 행동을 하는지 추상적으로 이해해서 그대로 표현해주고 내부적인 사정은 캡술화를 한다.)
>sort가 어떻게 동작하는지는 캡슐화로 숨겨야한다.
>
>task가 sort의 행동을 결정하기 때문이다.
>
>따라서 sort에 대응하는 함수 두개를 정한다. 또한 어떻게 동작하는지는 관심사가 아니기 때문에 캡슐화를 한다.



## 3.

```js
const TaskList = class {
  constructor(title){
    if(!title) throw 'invalid title'
    this._title = title
    this._list = []
  }
  add(title, date = Date.now()){ this._list.push( new Task(title, date) ) }
  remove(task) {
    const list = this._list;
    if(list.includes(task)) list.splice(list.indexOf(task),1)
  }
  byTitle(stateGroup = true) {
    return this._getList('title', stateGroup)
  }
  byDate(stateGroup = true) {
    return this._getList('date', stateGroup)
  }
  _getList(sort, stateGroup) {
    const list = this._list, s = taskSort[sort]
    return !stateGroup ? [...list].sort(s) : [
      ...list.filter(v => !v.isComplete()).sort(s),
      ...list.filter(v=>v.isComplete()).sort(s)
    ]
  }
}
```



## 4. 서브 Task 만들기

```js
const Task = class{
  constructor(title, date){
    if(!title) throw "invalid title"
    this._tilte = title
    this._date = date
    this._isComplete = false
    this._list = []
  }
  isComplete(){return this._isComplete}
  toggle(){this._isComplete = !this._isComplete}
  sortTitle(task){
    return this._tilte > task._title
  }
  sortDate(task){
    return this._date > task._date
  }
  add(title, date = Date.now()){ this._list.push( new Task(title, date) ) }
    remove(task) {
      const list = this._list;
      if(list.includes(task)) list.splice(list.indexOf(task),1)
    }
    byTitle(stateGroup = true) {
      return this._getList('title', stateGroup)
    }
    byDate(stateGroup = true) {
      return this._getList('date', stateGroup)
    }
    _getList(sort, stateGroup) {
      const list = this._list, s = taskSort[sort]
      return {
        task: this
        sub: !stateGroup ? [...list].sort(s) : [
        ...list.filter(v => !v.isComplete()).sort(s),
        ...list.filter(v=>v.isComplete()).sort(s)
      ]
    }
}

const TaskList = class {
  constructor(title){
    if(!title) throw 'invalid title'
    this._title = title
    this._list = []
  }
  add(title, date = Date.now()){ this._list.push( new Task(title, date) ) }
  remove(task) {
    const list = this._list;
    if(list.includes(task)) list.splice(list.indexOf(task),1)
  }
  byTitle(stateGroup = true) {
    return this._getList('title', stateGroup)
  }
  byDate(stateGroup = true) {
    return this._getList('date', stateGroup)
  }
  _getList(sort, stateGroup) {
    const list = this._list, s = taskSort[sort]
    return (!stateGroup ? [...list].sort(s) : [
      ...list.filter(v => !v.isComplete()).sort(s),
      ...list.filter(v=>v.isComplete()).sort(s)
    ]).map(v=> v._getList())
  }
}
```

>subtask가 하는 행위는 리스트클래스가 행하는 행위와 동일하다.
>
>TaskList는 이제 단순히 리스트를 반환하는것이 아니라 Task와 Sub의 개념으로 배뀌었다. 따라서List객체로 변환하여 sub object로 return한다.
>
>즉 TaskList는 실제로 오브젝트를 반환하는 역할을 하며, 그 오브젝트를 표현하는 역할은 TASK에서 Sub로 나타낸다.
>
>하지만 list안에 sublist가 또 존재하는 재귀적인 존재라면 이러한 하드코딩으로는 한계가 존재한다.
>
>데이터 코드를 살펴보자.
>
>```js
>{
>	items: 's75',
>	children: [{
>items: taskItem('3강 교안 작성'),
>children: [{
>	items: taskItem('코드 정리'),
>	children: []
>}]
>	}]
>}
>
>```
>
>items와 children이 중복되는것을 확인할 수 있다.



## 5. Task 클래스에 중복코드 합치기, 재귀적으로 구현하기

```js
const Task = class{
  static title(a, b){return a.sortTitle(b)}
  static date(a, b){return a.sortDate(b)}
  constructor(title) {
    if(!title) throw 'invalid title'
    else this._title = title
    this._list = []
  }
  add(task){
    if(task instanceof Task) this._list.push(task) 
    else throw 'invalid'
  }
  remove(task) {
    const list = this._list
    if(list.includes(task)) list.splice(list.indexOf(task),1)
  }
  getResult(sort, stateGroup){
    const list = this.list
    return {
      item: this._getResult(),
      children: (!stateGroup ? [...list].sort(sort) : [
        ...list.filter(v => v.isComplete()).sort(sort),
        ...list.filter(v => v.isComplete()).sort(sort)
      ]).map(v => v.getResult(sort, stateGroup))
    }
  }
  _getResult(){throw"override"}
  isComplete(){throw "override"}
  sortTitle(){throw "override"}
  sortDate(){throw "override"}
}
```

> getResult 메소드에서 children에 대한 getResult 메소드를 호출함으로써 재취적인 방식으로써 구현을 한다.(composite 패턴)

## 6. 하위코드 정리하기

```js
const TaskItem = class extends Task {
	constructor(title, date= Date.now()){
    super(title)
    this._date = date
    this._isComplete = false
  }
  _getResult(sort, stateGroup){return this}
  isComplete(){return this._isComplete}
  sortTitle(task){return this._title> task._title}
  sortDate(task){return this._date > task>_date}
  toggle(){this._isComplete = !this.isComplete}
}
const TaskList = class extends Task{
  constructor(title){
    super(title)
  }
  _getResult(){return this._title}
  isComplete(){}
  sortTitle(){return this}
  sortDate(){return this}
  byTitle(stateGroup = true){
    return this.getResult(Task.title, stateGroup)
  }
  byDate(stateGroup = true){
    return this.getResult(Task.date, stateGroup)
  }
}
```

> get는 부모쪽에서 호출하기 때문에 하위 데이터를 가져온다. 하지만 자기 자신의 데이터는 _get으로 상위에 전달해준다.
>
> html또한 데이터가 업데이트 되는 부분은 신경 쓸 필요없이 전제 데이터를 기준으로 업데이트해주기만 하면된다. 
>
> 따라서 부모가 누구인지만 알만 자식은 paddingLeftf를 통하여 밀어버리면 된다.
>
> 성능은 React등이 알아서 기존의 뷰와 변경될 뷰를 계산하여 렌더링해주므로 신경쓸 필요가 없다.

