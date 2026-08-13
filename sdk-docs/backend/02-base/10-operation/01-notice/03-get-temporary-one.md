---
sidebar_label: "임시공지 조회"
sidebar_position: "3"
description: "GetTempNotice"
---

# GetTempNotice

delegate void **TempNotice**(string **data**);  
public void **GetTempNotice**(TempNotice **tempNoticeCallback**);

## 설명

콘솔에서 등록한 임시 공지를 받아옵니다.  
해당 메소드는 비동기 형식만 제공됩니다.  

> callback의 형식이 BackendReturnObject가 아닌 string 형식입니다.  

:::info 안내
임시 공지는 뒤끝 데이터베이스가 아닌, json 파일 형태로 서버에 저장되기 때문에 뒤끝 access_token이 존재하지 않아도 사용할 수 있습니다.  
  
뒤끝 임시 공지의 사용목적은 다음과 같습니다.  
    
* 로그인하지 않은 회원에게 노출하고 싶은 정보가 있는 경우
* 서버 상태를 점검으로 한 후 유저에게 공지사항을 노출하고 싶은 경우
* 뒤끝 서버가 정상적으로 작동하지 않을 때 임시 공지를 노출하고자 하는 경우
:::

:::caution Unity 호환 버전 안내
해당 기능은 unity 2017.2 이상에서 정상적으로 작동합니다.  
:::

## Example

### 비동기

```js
Backend.Notice.GetTempNotice((callback) => {
  // 이후 처리
  Debug.Log(callback);
});
```

## ReturnCase

### Success cases

**임시 공지 불러오기에 성공한 경우**  

```js
{
    "isUse": true, // 사용 여부
    "contents": "기존 공지(Legacy) 작성 내용입니다.",
    "languageContents": {
        "en": "언어별 공지 영어(en) 작성 내용입니다.",
        "ko": "언어별 공지 한국어(ko) 작성 내용입니다."
    }
}
```

### Error cases

**임시공지를 설정한 적이 없는 경우**  
빈 스트링 값이 리턴됩니다.  
