---
sidebar_label: "로컬 초기화"
description: "로컬 초기화"
---

# Local.Reset
public void **Reset**();

## 설명
로컬에 저장된 모든 테이블을 삭제하고 초기화니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.
* 해당 함수는 동기방식만 지원하며 리턴값이 없습니다.

## Example
### 동기
```js
BackndGameTable.Instance.Local.Reset();
```
