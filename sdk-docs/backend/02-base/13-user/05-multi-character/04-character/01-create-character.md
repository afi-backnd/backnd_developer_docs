---
draft: "true"
unlisted: "true"
sidebar_label: "캐릭터 생성"
description: "CreateCharacter"
---

# CreateCharacter
public BackendReturnObject **CreateCharacter**(string **nickname**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| nickname   | string | 생성할 캐릭터의 닉네임 |

## 설명
해당 유저 계정에 새로운 캐릭터를 생성합니다. 닉네임은 중복이 불가능합니다.  
최대 20개까지 생성이 가능합니다.  
* 캐릭터 생성 성공 시, 리턴값으로 캐릭터 선택(SelectCharacter)에 필요한 inDate와 uuid가 표시되며, 해당 값을 이용하여 바로 캐릭터 선택하여 로그인할 수 있습니다.  

## Example

### 동기
```js

BackendReturnObject bro = Backend.MultiCharacter.Character.CreateCharacter("닉네임");
if(bro.IsSuccess()) {
  Debug.Log("캐릭터 생성에 성공했습니다.");
  Debug.Log("gamerInDate: " + bro.GetReturnValuetoJSON()["gamerInDate"].ToString());
  Debug.Log("uuid: " + bro.GetReturnValuetoJSON()["uuid"].ToString());
}
```

### 비동기
```js
BackendReturnObject bro = Backend.MultiCharacter.Character.CreateCharacter("닉네임", callback => {
  if(callback.IsSuccess()) {
      Debug.Log("계정 생성에 성공했습니다.");
      Debug.Log("gamerInDate: " + bro.GetReturnValuetoJSON()["gamerInDate"].ToString());
      Debug.Log("uuid: " + bro.GetReturnValuetoJSON()["uuid"].ToString());
  }
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.MultiCharacter.Character.CreateCharacter, "닉네임", callback => {
  if(callback.IsSuccess()) {
      Debug.Log("계정 생성에 성공했습니다.");
      Debug.Log("gamerInDate: " + bro.GetReturnValuetoJSON()["gamerInDate"].ToString());
      Debug.Log("uuid: " + bro.GetReturnValuetoJSON()["uuid"].ToString());
  }
});
```

## ReturnCase

### Success cases

**캐릭터 생성에 성공한 경우**  
statusCode : 201  
message : Success  
returnValue : `{"gamerInDate":"2023-06-19T07:54:08.420Z","uuid":"7869e640-0e76-11ee-866a-61533cb2837b"}`

### Error cases

**캐릭터가 20개를 초과했을 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden character count can not exceed 20, 금지된 character count can not exceed 20

**닉네임이 중복되는 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated nickname, 중복된 nickname 입니다

## SampleCode
다음 코드는 캐릭터 생성 후,  캐릭터를 선택하여 기본적인 커스텀마이징값을 저장하는 로직입니다.  
```js
Backend.MultiCharacter.Account.CreateAccount("user12", "user12");
var bro = Backend.MultiCharacter.Character.CreateCharacter("캐릭터2");
if(bro.IsSuccess() == false) {
	Debug.LogError("캐릭터 생성에 실패했습니다.");
	return;
}
LitJson.JsonData userData = bro.GetReturnValuetoJSON();
string uuid = userData["uuid"].ToString();
string inDate = userData["gamerInDate"].ToString();
bro = Backend.MultiCharacter.Character.SelectCharacter(uuid, inDate);

if(bro.IsSuccess() == false) {
	Debug.LogError("캐릭터 선택에 실패했습니다.");
	return;
}
// 캐릭터 커스터마이징
Param param = new Param();
param.Add("head", "default-1");
param.Add("clothes", "default-1");
param.Add("shoes", "default-2");
param.Add("class", "warrior");
param.Add("weapon", "BattleAXE");
bro = Backend.GameData.Insert("USER_DATA", param);
if(bro.IsSuccess()) {
	Debug.Log("캐릭터 초기화가 완료되었습니다.");
}
// 캐릭터화면으로 돌아갈 경우
Backend.BMember.Logout();
//SceneManager.LoadScene("characterScene");
// 그대로 게임을 시작할 경우
//SceneManager.LoadScene("ingameScene");
```
