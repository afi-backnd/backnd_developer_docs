---
description: "Between"
---

# Between
public void Between(string **key**, int **begin**, int **end**);  
public void Between(string **key**, long **begin**, long **end**);  
public void Between(string **key**, double **begin**, double **end**);

## 파라미터

| Value|  Type | Description |
| --- | --- | --|
| key | string| 컬럼의 key 값  |
| begin/end| int | (selection) 비교를 수행할 데이터  |
| begin/end| long | (selection) 비교를 수행할 데이터  |
| begin/end| double | (selection) 비교를 수행할 데이터  |

## 설명
key 컬럼의 데이터가 **begin**보다 같거나 크고 **end**보다 같거나 작은 데이터를 검색할 때 사용됩니다.  
> Key는 숫자로 시작할 수 없습니다. 숫자로 시작하는 경우, 경고가 출력되며 Where 추가되지 않습니다. 

## Example
```js
where.Between(string key, T begin, T end);
```
