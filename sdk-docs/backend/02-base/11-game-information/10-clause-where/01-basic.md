# where 절

게임 정보를 검색할 때 row의 inDate를 이용한 검색 이외에도 특정 컬럼 값을 통한 검색이 가능합니다.  
where 절을 이용한 검색은 Get, Update, Delete에 사용됩니다.  

where 절로 검색을 수행할 때는 테이블 내 존재하는 컬럼에 대해서만 검색이 가능합니다.  
유저 아이디, 닉네임 등을 이용한 검색은 할 수 없습니다.  
> 유저의 닉네임이 테이블 내 row에 존재하는 경우 닉네임을 통한 검색을 수행할 수 있습니다.  

### Where 메소드
Where 메소드는 다음과 같습니다.  
* Equal(key, value)
* NotEqual(key, value)
* Greater(key, value)
* GreaterOrEqual(key, value)
* Less(key, value)
* LessOrEqual(key, value)
* Between(key, begin, end)
* IsNull(key)
* IsNotNull(key)
* IsEmpty(key)
* IsNotEmpty(key)
* Contains(key, value)
* NotContains(key, value)
* BeginsWith(key, value)
* Clear()
* GetJson()
* GetValue()

### Example
``` js
// user 테이블에서 
// key 컬럼의 value가 backEnd이고 
// size 컬럼의 value가 15 이상이고 
// name 컬럼에 Guest가 포함된 테이블을 찾을 때
Where where = new Where();
where.Equal("key", "backEnd");
where.GreaterOrEqual("size", 15);
Where.Contains("name", "Guest");

Backend.GameData.Get("user", where, 10);
```
