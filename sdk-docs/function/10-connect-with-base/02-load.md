---
sidebar_label: 뒤끝베이스에서 보낸데이터 조회
---

# Content

## 설명

SDK에서 송신한 데이터는 `Content` 변수에 저장되어 있습니다.  
Content는 JsonData 타입의 변수로 간단한 방법으로 데이터를 조회할 수 있습니다.  

## Example

```js
// SDK에서 key가 mykey이고, value가 hello world인 데이터를 송신한 경우

string data = string.Empty;
if(Backend.HasKey("mykey") == true)
{
    data = Backend.Content["mykey"].ToString();
}
```
