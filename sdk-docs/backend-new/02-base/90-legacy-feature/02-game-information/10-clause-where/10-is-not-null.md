# IsNotNull
public void IsNotNull(string **key**);

## 파라미터

| Value|  Type | Description |
| --- | --- | --|
| key | string| 컬럼의 key 값  |


## 설명
key 컬럼의 데이터가 **null**이 아닌 데이터를 검색할 때 사용됩니다.  
> Key는 숫자로 시작할 수 없습니다. 숫자로 시작하는 경우, 경고가 출력되며 Where 추가되지 않습니다. 

## Example
```js
where.IsNotNull(string key);
```