---
sidebar_label: 문의 보내기
description: "문의 보내기"
sidebar_position: 3
---

# SendInquiry

public Task&lt;RequestResult&gt; **SendInquiryAsync**(InquiryType **inquiryType**, string **title**, string **content**);

## 파라미터

| Value        | Type         | Description |
| :----------- | :----------- | :---------- |
| inquiryType  | InquiryType  | 문의 유형   |
| title        | string       | 문의 제목   |
| content      | string       | 문의 내용   |

### InquiryType

| Type                        | Description |
| :-------------------------- | :---------- |
| InquiryType.Account        | 계정        |
| InquiryType.Payment        | 결제        |
| InquiryType.Restore        | 복구        |
| InquiryType.GameQuestion   | 게임문의    |
| InquiryType.Report         | 신고        |
| InquiryType.SetupOrExecute | 설치/진행   |
| InquiryType.Bug            | 버그        |
| InquiryType.Event          | 이벤트      |
| InquiryType.Suggestions    | 건의사항    |
| InquiryType.Etc            | 기타        |

## 설명

문의를 발송합니다. 문의 유형은 enum InquiryType으로 지정할 수 있습니다.  

> 사전 업로드 기능은 현재 제공하고 있지 않습니다.  

## Example

### Task 형식
```js
var reqResult = await BackndInquiry.Instance.SendInquiryAsync(InquiryType.Account, "제목", "내용");
```

### Callback 형식
```js
BackndInquiry.Instance.SendInquiry(InquiryType.Account, "제목", "내용", (callback) =>
{            
});
```

## Return Cases

### Success Cases

statusCode : 204  
message : Success  
