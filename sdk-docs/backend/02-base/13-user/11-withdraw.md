---
sidebar_label: "회원 탈퇴"
description: "WithdrawAccount"
---

# WithdrawAccount
public BackendReturnObject **WithdrawAccount**();  
public BackendReturnObject **WithdrawAccount**(int graceHours);

## 파라미터

| Value        | Type           | Description  | default |
| :------------ |:------------| :-----|:-----|
| graceHours | int     | 탈퇴 유예 기간 |     0(즉시탈퇴)   |

:::danger Sign in with Apple 이용 시 주의사항 안내  
[애플 심사 정책](https://developer.apple.com/kr/news/?id=12m75xbj)에 따라 Sign in with Apple 소셜 로그인이 구현되어있는 앱의 경우, 회원 탈퇴 시, Apple 서버에 해당 유저에 대한 토큰 취소 REST API를 호출해야합니다.  
  
SDK 5.11.9 버전부터 애플 서버에서 authorizationCode를 받아 콘솔에 입력한 정보와 함께 애플 토큰 만료 REST API를 호출하는 함수가 추가되었습니다.  
Sign in with Apple을 구현하신 경우 SDK 5.11.9 이상 버전으로 업데이트하여 **[애플 토큰 만료 기능](/sdk-docs/backend/base/user/revoke-apple)**을 꼭 구현하신 후 이용해 주세요.  
:::  

## 설명
서버에서 회원 탈퇴를 진행합니다.  
기존 SignOut 함수와의 차이점은 다음과 같습니다.  

### 기존 SignOut 함수와의 차이점
<table>
  <tr>
    <th> </th>
    <th>SignOut</th>
    <th>WithdrawAccount</th>
  </tr>
  <tr>
    <td>유예 기간</td>
    <td>7일 고정</td>
    <td>한 시간 단위로 설정 가능</td>
  </tr>
  <tr>
    <td>탈퇴 취소</td>
    <td>로그인 시 탈퇴 철회</td>
    <td>즉시 탈퇴일 경우, 정시 이전까지 콘솔에서만 탈퇴 철회 가능.  
로그인 시 410 에러 발생.  
유예 시간이 있을 경우 로그인 시 탈퇴 철회.</td>
  </tr>
  <tr>
    <td>데이터 삭제</td>
    <td>푸시, 길드, 친구, 랭킹, 닉네임, 페더레이션, 국가 코드</td>
    <td>SignOut()의 데이터 삭제 + 내 모든 게임 정보 관리 데이터</td>
  </tr>
</table>

탈퇴 시, 다음 정보들이 자동으로 삭제됩니다.  
* 푸시에 쓰이는 디바이스 토큰 삭제
* 친구 정보(친구 맺기 요청 리스트 삭제, 친구 맺기 요청 받은 리스트 삭제) 삭제
* 랭킹 정보 삭제
* 닉네임 정보 삭제
* 페더레이션 & 커스텀 아이디 정보 삭제
* 국가 코드 정보 삭제
* 게임 정보 삭제
* 길드 정보 삭제  
- 길드장이 탈퇴 시, 해당 길드는 길드장이 존재하지 않게 됩니다.  
- 길드에 해당 유저만 존재할 경우, 길드는 자동으로 삭제됩니다.  

### 즉시 탈퇴
인자값을 입력하지 않거나 graceHours가 0일 경우 즉시 탈퇴가 이루어집니다.  
호출 후 해당 유저에 관한 모든 정보가 즉시 삭제되지 않으며 다음과 같이 진행됩니다.  

1. 호출 성공 시 해당 유저는 강제로 로그아웃 상태가 됩니다.(이후 뒤끝 함수 요청시 bad accesstoken 에러 발생)

2. 이후 로그인 함수 호출 시, 다음과 같은 에러가 출력되며 로그인이 불가능합니다.  
> statusCode : 410  
errorCode : GoneResourceException  
message : Gone user, 사라진 user 입니다

3. 데이터는 바로 삭제되지 않고, 호출 시간과 가장 가까운 시간 정시에 전부 삭제됩니다.  
(10시 15분에 즉시 탈퇴 요청 시, 요청 후 로그인이 불가능하며 11시에 데이터가 삭제됩니다.)

4. 탈퇴 요청 후 유저가 탈퇴를 철회하는 것은 불가능하며, 콘솔에서 데이터 삭제 전에 탈퇴여부을 정상으로 변경할 경우 탈퇴되지 않습니다.  

탈퇴 요청 이후 바로 해당 아이디와 동일한 아이디를 생성하는 것은 불가능하며, 데이터가 삭제된 후에 생성할 수 있습니다.  
유저들에게는 탈퇴 요청 후 탈퇴 완료까지 최대 1시간이 소요될 수 있다고 안내해 주시기 바랍니다.  

### 예약 탈퇴
graceHours를 입력할 경우 해당 시간 만큼의 유예 시간이 주어집니다.  
탈퇴 요청 후, 유예 시간 안에 로그인 시 탈퇴 요청이 취소됩니다.  
유예 시간이 지난 후 즉시 탈퇴와 동일하게 로그인 불가 상태가 되며, 가장 가까운 시간 정시에 데이터가 삭제됩니다.  

10시 15분에 graceHours가 2인 탈퇴 요청을 호출할 경우 다음과 같습니다.  

1. 탈퇴 요청 후에 뒤끝 함수 호출 시 bad accesstoken 에러 발생.  

2. 12시 15분 이전에 로그인 시, 로그인이 성공하며 탈퇴 요청이 철회됩니다.  

3. 12시 15분 이후에 로그인 시, 다음과 같은 에러가 출력되며 로그인이 불가능합니다.  
> statusCode : 410  
errorCode : GoneResourceException  
message : Gone user, 사라진 user 입니다

4. 12시 15분과 가장 가까운 1시에 해당 유저에 대한 모든 데이터 삭제가 발생합니다.  



## Example

### 동기
```js
//즉시 탈퇴
Backend.BMember.WithdrawAccount();

//2시간 뒤에 탈퇴 예약
Backend.BMember.WithdrawAccount(2);


//7일 뒤에 탈퇴 예약(기존 SignOut 함수)
Backend.BMember.WithdrawAccount(24 * 7);
```

### 비동기
```js
//즉시 탈퇴
Backend.BMember.WithdrawAccount(callback  => {
    // 이후 처리
});

//2시간 뒤에 탈퇴 예약
Backend.BMember.WithdrawAccount(2, callback => {
    // 이후 처리
});
```

### SendQueue
```js
//즉시 탈퇴
SendQueue.Enqueue(Backend.BMember.WithdrawAccount, callback => {
    // 이후 처리
});

//2시간 뒤에 탈퇴 예약
SendQueue.Enqueue(Backend.BMember.WithdrawAccount, 2, callback => {
    // 이후 처리
});
```

## ReturnCase

### Success cases
**회원 탈퇴에 성공한 경우**  
statusCode : 204  
message : Success
