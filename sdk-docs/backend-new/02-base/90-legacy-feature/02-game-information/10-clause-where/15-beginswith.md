# BeginsWith
public void BeginsWith(string **key**, string **value**);

## 파라미터

| Value|  Type | Description |
| --- | --- | --|
| key | string| 컬럼의 key 값  |
| value | string | 비교를 수행할 데이터 타입  |


## 설명
key 컬럼의 데이터에 **value**문자열로 시작하는 데이터를 검색할 때 사용됩니다.  
> Key는 숫자로 시작할 수 없습니다. 숫자로 시작하는 경우, 경고가 출력되며 Where 추가되지 않습니다. 

## Example
```js
where.BeginsWith(string key, string value);
```