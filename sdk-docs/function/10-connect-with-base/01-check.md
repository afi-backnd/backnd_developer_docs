---
sidebar_label: 뒤끝베이스에서 보낸데이터확인
---

# HasKey

public bool HasKey(string key);

## 파라미터

| Value | Type   | Description                     |
| ----- | ------ | ------------------------------- |
| key   | string | 확인하고자 하는 데이터의 key 값 |

## 설명

SDK(뒤끝 베이스)에서 보낸 데이터를 확인합니다.  
확인하고자 하는 데이터의 key 값을 기입하면 해당 데이터가 존재하는지 bool 값으로 리턴합니다.  

## Example

```js
bool isExist = Backend.HasKey("myKey");
```

## ReturnCase

**SDK에서 송신한 데이터에 해당 키가 존재할 때**  
true

**SDK에서 송신한 데이터에 해당 키가 존재하지 않을 때**  
false
