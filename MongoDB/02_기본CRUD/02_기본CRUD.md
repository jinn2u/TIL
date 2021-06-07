### 삽입

```
> db.users.insert({username: "jones"})
WriteResult({ "nInserted" : 1 })
> db.users.insert({username: "smith"})
WriteResult({ "nInserted" : 1 })
```

### ID 필드

```
> db.users.count() 
2
> db.users.find()
{ "_id" : ObjectId("60b3977069c9e31f4cf14748"), "username" : "smith" }
{ "_id" : ObjectId("60b3990d69c9e31f4cf14749"), "username" : "jones" }
```



### AND 연산자

```
// 여러개의 필드를 명시하여 and를 할수 있다.
> db.users.find({_id: ObjectId("60b3990d69c9e31f4cf14749"), username: "jones"}) 

// 또는 명시적으로 할 수 있다.
> db.users.find({$and: [
... {_id: ObjectId("60b3990d69c9e31f4cf14749")},
... {username: "jones"}
... ]})
```

### OR 연산자

```
> db.users.find({$or: [
... {username: "smith"},
... {username: "jones"}
... ]})
```

### 도큐먼트 업데이트

- 모든 업데이트 쿼리는 최소 두 개의 매개변수가 필요하다.
- $set 연산자는 단일 필드를 명시된 값으로 설정한다.

```
> db.users.update({username: "smith"}, {$set:{country: "Canada"}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

> db.users.find({username: "smith"})
{ "_id" : ObjectId("60b3977069c9e31f4cf14748"), "username" : "smith", "country" : "Canada" }
```

#### 대체 업데이트

- 필드값을 설정하는 것이 아니라 아예 도큐먼트를 다른 것으로 대체하는것이다.

- 이는 첫번째 도큐먼트가 오직 매칭을 위해서만 사용되었기 때문이다.

  ```
  > db.users.update({username: "smith"}, {country: "Canada"})
  WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
  
  > db.users.find({country:"Canada"})
  { "_id" : ObjectId("60b3977069c9e31f4cf14748"), "country" : "Canada" }
  ```

  - 값을 지우기 위해서는 $unset연산자를 쓸 수 있다.

    ```
    > db.users.update({username:"smith"}, {$unset:{country:1}})
    WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
    > db.users.find()
    { "_id" : ObjectId("60b3977069c9e31f4cf14748"), "username" : "smith" }
    { "_id" : ObjectId("60b3990d69c9e31f4cf14749"), "username" : "jones" }
    ```

    

  ```
  > db.users.find().pretty()
  {
  	"_id" : ObjectId("60b3977069c9e31f4cf14748"),
  	"username" : "smith",
  	"favorites" : {
  		"cities" : [
  			"chicago",
  			"newyork"
  		],
  		"movies" : [
  			"casablanca",
  			"for a few dollars more",
  			"the sting"
  		]
  	}
  }
  {
  	"_id" : ObjectId("60b3990d69c9e31f4cf14749"),
  	"username" : "jones",
  	"favorites" : {
  		"movies" : [
  			"casablanca",
  			"rocky"
  		]
  	}
  }
  ```

### 더 발전된 업데이트

- smith와 jones의 영화 변경하기

- $set연산의 경우 배열 전체에 대해 다시 쓰기를 해야한다. 
- 따라서 $push나 $addToSet을 사용하는것이 더 낫다. 
- 두 연산자 모두 배열에 아이템을 추가하지만 $addToSet은 값을 추가할 때 중복되지 않도록 확인한다.

```
> db.users.update({"favorites.movies": "Casablanca"},
... {$addToSet: {"favorites.movies": "The Maltese Falcon"}},
... faslse,
... true)
```

- 첫번째 인자: 쿼리셀렉터
- 두번째 인자: $addToSet연선자를 이용하여 리스트에 추가
- 세번째 인자: upsert가 허용되는지 조절한다.
  - 해당되는 도큐먼트가 존재하지 않을 때 update연산이 도큐먼트를 입력해야 하는지 아닌지를 말해준다.
  - 업데이트가 연잔자 업데이트인지 대체를 위한 업데이트인지에 따라 다른 행동을 취한다.
- 하나 이상의 도큐먼트에 대해 업데이트가 이루어져야함을 뜻한다.
  - 쿼리셀렉터의 조건에 맞는 첫 번재 도큐머트에 대해서만 업데이트를 하는 것이 기본 설정이다.
  - 따라서 모든 도큐먼트에 대해 연산을 수행하려먼 명시적으로 지정해야 한다.
  - smith와 jones 모두 업데이트를 해야 되므로 다중 업데이트가 필요하다.



### 데이터 삭제

- 도큐먼트 지우기
- remove는 컬렉션을 지우는것이 아니라 도큐먼트를 지우는것이다.(SQL delete와 유사하다.)

```
> db.foo.remove({})
> db.users.remove({"favorites.cities": "Cheyenne"})
```

### 컬렉션 삭제

```
> db.users.drop()
```

