---
sidebar_label: 로컬에 저장된 게스트 ID 조회
---

# GetGuestID
public string **GetGuestID**();

## 설명
기기 내부에 저장된 게스트 아이디를 조회합니다.  

## Example
```js
string id = Backend.BMember.GetGuestID();
Debug.Log("로컬 기기에 저장된 아이디 :" + id);
```
