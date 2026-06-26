---
sidebar_label: 유저 정보 조회(로컬)
---

# 로컬에 저장된 유저 정보 조회

public string **UserInDate**;  
public string **UserNickName**;  

## 설명

로컬에 저장되어 있는 유저의 inDate와 닉네임을 조회합니다.  
회원가입/로그인 한 후부터 확인할 수 있습니다.  
현재 뒤끝베이스에 로그인되어 있는 유저의 inDate와 닉네임이 리턴됩니다.  

> 새로운 계정으로 회원가입/로그인할 때마다 갱신됩니다.  

## Example

```js
string inDate = Backend.UserInDate;
string nickName = Backend.UserNickName;

Debug.Log(inDate);
Debug.Log(nickName);
```

## Return cases

**로그인한 후 변수에 접근한 경우**  
UserInDate : 현재 로그인 된 계정의 유저 inDate가 리턴됩니다.  
UserNickName : 현재 로그인 된 계정의 유저 닉네임이 리턴됩니다.  

**닉네임이 없는 유저로 로그인한 후 변수에 접근한 경우**  
UserInDate : 현재 로그인 된 계정의 유저 inDate가 리턴됩니다.  
UserNickName : 빈 string 객체가 리턴됩니다.(string.Empty)

**회원가입 직후 변수에 접근한 경우(닉네임이 존재하지 않은 경우와 동일)**  
UserInDate : 현재 로그인 된 계정의 유저 inDate가 리턴됩니다.  
UserNickName : 빈 string 객체가 리턴됩니다.(string.Empty)

**닉네임 생성/수정 후 변수에 접근한 경우**  
UserInDate : 현재 로그인 된 계정의 유저 inDate가 리턴됩니다.  
UserNickName : 현재 로그인 된 계정의 유저 닉네임이 리턴됩니다.(변경된 닉네임으로 리턴됩니다.)

**로그인하기 전 혹은 로그인 실패 후 변수에 접근한 경우**  
UserInDate : 빈 string 객체가 리턴됩니다.(string.Empty)
UserNickName : 빈 string 객체가 리턴됩니다.(string.Empty)
