---
sidebar_label: 로컬에 저장된 게스트 정보 삭제
---

# DeleteGuestInfo
public void **DeleteGuestInfo**();

## 설명
기기 내부에 저장된 게스트 계정 정보를 삭제합니다.  
> 로컬에 저장되어 있는 게스트 계정 정보를 삭제할 뿐이고, 실제 서버에 저장되어 있는 계정 정보는 삭제되지 않습니다.  

## Example
```js
BackndAuth.Instance.DeleteGuestInfo();
```
