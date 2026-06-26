---
sidebar_label: Null 삽입
---

# AddNull
public void **AddNull**(string **key**);

## 파라미터

| Value|  Type | Description |
| --- | --- | --|
| key | string| 키값 |

## 설명
Param에 빈 컬럼을 추가합니다.  
* **Key는 숫자로 시작할 수 없습니다.** 숫자로 시작하는 경우, 경고가 출력되며 Param에 추가되지 않습니다.  


## Example

```js
Param param = new Param();

param.AddNull(string key);
```
