---
sidebar_label: "로컬 초기화"
---

# Backend.CDN.Content.Local.Reset
public void **Backend.CDN.Content.Local.Reset**();

## 설명
로컬에 저장된 모든 차트를 삭제하고 초기화니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.
* 해당 함수는 동기방식만 지원하며 리턴값이 없습니다.

## Example
### 동기
```js
Backend.CDN.Content.Local.Reset();
```
