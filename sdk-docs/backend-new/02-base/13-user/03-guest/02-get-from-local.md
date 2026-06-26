---
sidebar_label: 로컬에 저장된 게스트 ID 조회
description: "로컬에 저장된 게스트 ID 조회"
---

# GetGuestId
public string **GetGuestId**();

## 설명
기기 내부에 저장된 게스트 아이디를 조회합니다.  

## Example
```js
string id = BackndAuth.Instance.GetGuestId();
Debug.Log("로컬 기기에 저장된 아이디 :" + id);
```
