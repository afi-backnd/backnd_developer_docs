---
sidebar_label: "이메일 정보 등록"
description: "UpdateFederationEmail"
---

# UpdateFederationEmail
public BackendReturnObject **UpdateFederationEmail**(string **federationToken**, FederationType **type**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| federationToken      | string | 페더레이션 token |
| type      | FederationType(enum) | 페더레이션의 종류.<br />`FederationType.Google`<br />`FederationType.Facebook`<br/>`FederationType.Apple` |

## 설명
기존의 페더레이션 가입한 회원들 중 email 수집 안된 회원들의 이메일 정보를 저장합니다.  

## Example

### 동기
```js
Backend.BMember.UpdateFederationEmail("federationToken" , FederationType.Google);
```

### 비동기
```js
Backend.BMember.UpdateFederationEmail("federationToken", FederationType.Google, (callback) =>
{
    // 이후 처리
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.UpdateFederationEmail, "federationToken", FederationType.Google, (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**이메일 정보 업데이트에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**유저 정보가 뒤끝 데이터베이스에 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : federationId not found, federationId을(를) 찾을 수 없습니다
