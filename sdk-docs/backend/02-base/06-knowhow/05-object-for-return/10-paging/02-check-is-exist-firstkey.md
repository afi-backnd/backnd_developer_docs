---
sidebar_label: firstKey 존재여부 확인
---

# HasFirstKey
public bool **HasFirstKey**();  
public bool **HasFirstKey**(out string **firstKey**);

| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| firstKey | string | (optional) firstKey가 존재하는 경우 전달된 인수에 string형태의 firstKey를 할당합니다. | 

## 설명
returnValue에 firstKey가 존재하는지 확인합니다.  

## Example
``` js
BackendReturnObject bro = Backend.GameData.Get("tableName", new Where());

string firstKey = null;
if(bro.HasFirstKey()) {
    firstKey = bro.FirstKeyString();
}

if(bro.HasFirstKey(out firstKey)) {
    Debug.Log(firstKey);
}
```

## Return Cases
**returnValue에 firstKey가 존재하는 경우**  
return : true  
firstKey : firstKey가 할당됩니다.  

**returnValue에 firstKey가 존재하지 않는 경우**  
return : false  
firstKey : string.Empty  

**returnValue가 존재하지 않는 경우**  
return : false  
firstKey : string.Empty  
