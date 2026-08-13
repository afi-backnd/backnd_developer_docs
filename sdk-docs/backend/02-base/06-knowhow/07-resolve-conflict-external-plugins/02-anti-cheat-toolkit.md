---
description: "Anti-Cheat Toolkit"
---

# Anti-Cheat Toolkit

## Anti-Cheat Toolkit에서 제공하는 기능과 충돌 문제 해결

Anti-Cheat Toolkit에서 제공하는 **Obscured 변수를 Param에 Add 한 후 해당 Param 값을 이용하여 데이터 생성/수정을 시도한 경우 {} 값이 저장될 수 있습니다.**  

Obscurd 변수를 Param에 Add 할 때는 복호화 된 값을 Param에 Add 해야 정상적인 값을 서버에 저장할 수 있습니다.  

### 복호화 된 값 리턴하기
**GetDecryped()**  

## Example
```js
ObscuredInt level = 5;
Param param = new Param();

// 이렇게 그대로 사용하면 {} 값이 저장됩니다.(이렇게 사용하면 안 됩니다.)
param.Add("level", level);

// 이렇게 사용해야 정상적인 값이 저장됩니다.  
param.Add("level", level.GetDecrypted());
```
