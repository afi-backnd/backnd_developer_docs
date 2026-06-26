---
sidebar_label: "문의 기본양식 불러오기"
description: "문의 기본양식 불러오기"
sidebar_position: 2
---

# GetDefaultInquiryForm

public Task&lt;GetDefaultInquiryFormResult&gt; **GetDefaultInquiryFormAsync**();

## 설명

뒤끝 콘솔의 `1:1 문의 > 문의 설정`에서 설정한 문의 양식을 불러옵니다.  
문의 양식이 설정되어 있지 않을 경우 `""`(string.Empty)으로 리턴됩니다.  

유저의 언어 정보를 통해 콘솔에서 등록된 기본 폼 양식 중 국가가 일치하는 양식으로 불러옵니다.  


## Example

### Task 형식
```js
var reqResult = await BackndInquiry.Instance.GetDefaultInquiryFormAsync();
var defaultForm = reqResult.GetForm();
```

### Callback 형식
```js
BackndInquiry.Instance.GetDefaultInquiryForm((callback) =>
{
    var defaultForm = reqResult.GetForm();
});
```

## ReturnCase

### Success cases

**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON

```js
{
    "type":["계정","결제","복구","게임문의","신고","설치/실행","버그","이벤트","건의사항","기타"],
    "form":"문제 발생 시간을 기재해 주세요!"
}
```
