---
draft: "true"
unlisted: "true"
sidebar_label: "캐릭터 삭제"
description: "DeleteCharacter"
---

# DeleteCharacter

public BackendReturnObject **DeleteCharacter**(string **uuid**, string **inDate**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| uuid | string | 소유중인 캐릭터의 uuid |
| inDate | string | 소유중인 캐릭터의 inDate |

## 설명
입력한 uuid와 inDate에 일치하는 내 소유 캐릭터를 삭제합니다.  
삭제 시,  해당 캐릭터의 다음 정보들 또한 삭제됩니다.  

다음 정보는 즉시 삭제됩니다.  
* 푸시에 쓰이는 디바이스 토큰 삭제
* 친구 정보(친구 맺기 요청 리스트 삭제, 친구 맺기 요청 받은 리스트 삭제) 삭제

다음 정보는 탈퇴를 신청했을 때의 가장 가까운 정각이 지난후에 삭제가 됩니다.  
(12시 30분에 탈퇴를 신청할 경우, 1시에 데이터가 삭제됩니다.)
* 랭킹 정보 삭제
* 닉네임 정보 삭제
* 페더레이션 & 커스텀 아이디 정보 삭제
* 국가 코드 정보 삭제
* 게임 정보 삭제
* 길드 정보 삭제  - 길드 마스터이 탈퇴 시, 해당 길드는 길드 마스터이 존재하지 않게 됩니다.  - 길드에 해당 유저만 존재할 경우, 길드는 자동으로 삭제됩니다.  


## Example

### 동기
```js
var bro = Backend.MultiCharacter.Character.GetCharacterList();

// 0번째 유저
LitJson.JsonData characterJson = bro.GetReturnValuetoJSON()["characters"][0];

string uuid = characterJson["uuid"].ToString();
string inDate = characterJson["inDate"].ToString();

var bro2 = Backend.MultiCharacter.Character.DeleteCharacter(uuid, inDate);
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

      Backend.MultiCharacter.Character.DeleteCharacter(uuid, inDate, callback2 => {
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

      SendQueue.Enqueue(Backend.MultiCharacter.Character.DeleteCharacter, uuid, inDate, callback2 => {
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

**캐릭터 삭제에 성공한 경우**  
statusCode : 204  
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
