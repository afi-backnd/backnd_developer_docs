---
sidebar_label: "캐릭터 로그인"
description: "SelectCharacter"
---

# SelectCharacter

public BackendReturnObject **SelectCharacter**(string **uuid**, string **inDate**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| uuid | string | 소유중인 캐릭터의 uuid |
| inDate | string | 소유중인 캐릭터의 inDate |

## 설명
uuid와 inDate가 일치하는 내 캐릭터로 로그인을 시도합니다.  
성공일 경우, 로그인이 된 상태이며 뒤끝베이스 호출이 가능해집니다.  

## Example

### 동기
```js
var bro = Backend.MultiCharacter.Character.GetCharacterList();

// 0번째 유저
LitJson.JsonData characterJson = bro.GetReturnValuetoJSON()["characters"][0];

string uuid = characterJson["uuid"].ToString();
string inDate = characterJson["inDate"].ToString();

var bro2 = Backend.MultiCharacter.Character.SelectCharacter(uuid, inDate);

if(bro2.IsSuccess()) {
    Debug.Log("로그인에 성공했습니다");
}
else {
 Debug.LogError("로그인에 실패했습니다 " + bro2.ToString());
}
```

### 비동기
```js
Backend.MultiCharacter.Character.GetCharacterList(callback => {
  if(callback.IsSuccess()) {
      // 0번째 유저
      LitJson.JsonData characterJson = callback.GetReturnValuetoJSON()["characters"][0];

      string uuid = characterJson["uuid"].ToString();
      string inDate = characterJson["inDate"].ToString();

      Backend.MultiCharacter.Character.SelectCharacter(uuid, inDate, callback2 => {
            if(callback2.IsSuccess()) {
                Debug.Log("삭제하였습니다 : " + callback2.ToString());
            }
            else {
                Debug.LogErorr("삭제에 실패했습니다 : " + callback2.ToString());
            }
      );
  }
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.MultiCharacter.Character.GetCharacterList, callback => {
  if(callback.IsSuccess()) {
      // 0번째 유저
      LitJson.JsonData characterJson = callback.GetReturnValuetoJSON()["characters"][0];

      string uuid = characterJson["uuid"].ToString();
      string inDate = characterJson["inDate"].ToString();

      SendQueue.Enqueue(Backend.MultiCharacter.Character.SelectCharacter, uuid, inDate, callback2 => {
            if(callback2.IsSuccess()) {
                Debug.Log("삭제하였습니다 : " + callback2.ToString());
            }
            else {
                Debug.LogErorr("삭제에 실패했습니다 : " + callback2.ToString());
            }
      );
  }
});
```

## ReturnCase

### Success cases

**로그인에 성공한 경우**  
statusCode : 200  
message : Success  

### Error cases

**uuid 혹은 inDate가 null 혹은 string.Empty일 경우**  
statusCode : 400  
errorCode : ValidationException  
message : uuid is null or empty 혹은 inDate is null or empty

**uuid와 inDate가 일치하지 않을 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gamer not found, gamer을(를) 찾을 수 없습니다
