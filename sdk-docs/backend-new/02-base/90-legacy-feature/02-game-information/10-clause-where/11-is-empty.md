---
description: "IsEmpty"
---

# IsEmpty
public void IsEmpty(string **key**);

## 파라미터

| Value|  Type | Description |
| --- | --- | --|
| key | string| 컬럼의 key 값  |


## 설명
key 컬럼의 데이터가 존재하지 않는 데이터를 검색할 때 사용됩니다.  
* key 컬럼의 데이터가 null인 경우를 검색하기 위해서는 IsNull 함수를 사용해 주세요.  
* 해당 함수를 사용하면 데이터가 null인 row는 검색되지 않습니다.  
> Key는 숫자로 시작할 수 없습니다. 숫자로 시작하는 경우, 경고가 출력되며 Where 추가되지 않습니다. 

## Example
```js
where.IsEmpty(string key);
```
