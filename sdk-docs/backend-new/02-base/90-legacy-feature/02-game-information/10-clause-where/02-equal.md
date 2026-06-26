# Equal
public void Equal(string **key**, string **value**);  
public void Equal(string **key**, int** value**);  
public void Equal(string **key**, long **value**);  
public void Equal(string **key**, double **value**);  
public void Equal(string **key**, bool **value**);

## 파라미터

| Value|  Type | Description |
| --- | --- | --|
| key | string| 컬럼의 key 값  |
| value | string| (selection) 비교를 수행할 데이터  |
| value | int | (selection) 비교를 수행할 데이터  |
| value | long | (selection) 비교를 수행할 데이터  |
| value | double | (selection) 비교를 수행할 데이터  |
| value | bool | (selection) 비교를 수행할 데이터  |


## 설명
key컬럼의 데이터가 **value**와 일치하는 데이터를 검색할 때 사용됩니다.  
> Key는 숫자로 시작할 수 없습니다. 숫자로 시작하는 경우, 경고가 출력되며 Where 추가되지 않습니다. 

## Example
```js
where.Equal(string key, T value);
```
