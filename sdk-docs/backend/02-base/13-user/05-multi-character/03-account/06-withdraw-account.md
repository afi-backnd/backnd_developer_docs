---
sidebar_label: 계정 탈퇴
---

# WithdrawAccount
public BackendReturnObject **WithdrawAccount**();  
public BackendReturnObject **WithdrawAccount**(int expirationHours);

## 파라미터

| Value        | Type           | Description  | default |
| :------------ |:------------| :-----|:-----|
| expirationHours | int  | 탈퇴 유예 기간 |     0(즉시탈퇴)   |

## 설명
접속중인 유저 계정의 회원 탈퇴를 진행합니다.  
탈퇴 시 해당 유저가 소유중인 캐릭터들도 모두 삭제가 됩니다.  
또한 기기에 저장된 액세스토큰과 리프레시 토큰은 삭제되어 자동 로그인이 불가능해지며, 로그아웃됩니다.  

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
expirationHours를 입력할 경우 해당 시간 만큼의 유예 시간이 주어집니다.  
탈퇴 요청 후, 유예 시간 안에 로그인 시 탈퇴 요청이 취소됩니다.  
유예 시간이 지난 후 즉시 탈퇴와 동일하게 로그인 불가 상태가 되며, 가장 가까운 시간 정시에 데이터가 삭제됩니다.  

10시 15분에 expirationHours가 2인 탈퇴 요청을 호출할 경우 다음과 같습니다.  

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
Backend.MultiCharacter.Account.WithdrawAccount();

//2시간 뒤에 탈퇴 예약
Backend.BMember.MultiCharacter.Account.WithdrawAccount(2);
```

### 비동기
```js
//즉시 탈퇴
Backend.MultiCharacter.Account.WithdrawAccount(callback  => {
    // 이후 처리
});

//2시간 뒤에 탈퇴 예약
Backend.MultiCharacter.Account.WithdrawAccount(2, callback => {
    // 이후 처리
});
```

### SendQueue
```js
//즉시 탈퇴
SendQueue.Enqueue(Backend.MultiCharacter.Account.WithdrawAccount, callback => {
    // 이후 처리
});

//2시간 뒤에 탈퇴 예약
SendQueue.Enqueue(Backend.MultiCharacter.Account.WithdrawAccount, 2, callback => {
    // 이후 처리
});
```

## ReturnCase

### Success cases
**회원 탈퇴에 성공한 경우**  
statusCode : 204  
message : Success
