# GreaterOrEqual
public void GreaterOrEqual(string **key**, int **value**);  
public void GreaterOrEqual(string **key**, long **value**);  
public void GreaterOrEqual(string **key**, double **value**);

## 파라미터

| Value|  Type | Description |
| --- | --- | --|
| key | string| 컬럼의 key 값  |
| value | int | (selection) 비교를 수행할 데이터  |
| value | long | (selection) 비교를 수행할 데이터  |
| value | double | (selection) 비교를 수행할 데이터  |

## 설명
key 컬럼의 데이터가 **value**보다 같거나 큰 데이터를 검색할 때 사용됩니다.  
> Key는 숫자로 시작할 수 없습니다. 숫자로 시작하는 경우, 경고가 출력되며 Where 추가되지 않습니다. 

## Example
```js
where.GreaterOrEqual(string key, T value);
```
