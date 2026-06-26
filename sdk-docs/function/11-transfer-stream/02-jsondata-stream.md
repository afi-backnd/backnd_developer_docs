---
sidebar_label: JsonData를 Stream으로 변환
---

# JsonDataToStream

public Stream JsonToStream(string **text**);

## 파라미터

| Value | Type     | Description                       |
| ----- | -------- | --------------------------------- |
| json  | JsonData | stream으로 변환을 원하는 JsonData |

## 설명

JsonData 타입의 데이터를 stream으로 변환합니다.  

## Example

```js
var jsonVal = Backend.BMember.GetUserInfo();

var streamData = Backend.JsonToStream(jsonVal.GetReturnValuetoJSON());
```

## ReturnCase

stream 이 리턴됩니다.  
