---
sidebar_label: "임의의 객체를 Param으로 변환"
description: "Parse"
---

# Parse
static public Param **Parse**(string **jsonString**);  
static public Param **Parse**(LitJson.JsonData **jsonData**);  
static public Param **Parse**(T **value**);  

## 파라미터

| 변수명 | 자료형 | 설명  |
| :------------ |:------------| :-----|
| jsonString | string | json 형태로 저장된 string 데이터를 Param 형태로 파싱 합니다. |
| jsonData | JsonData | jsonData 데이터를 Param 형태로 파싱 합니다. |
| value | T(레퍼런스 타입 제너릭) | 개발자가 선언한 클래스를 Param 형태로 파싱 합니다. |
> 클래스 내 private 변수는 파싱 할 때 포함되지 않습니다.  
private 변수 중 param에 추가하고 싶은 값은 [property(속성)](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/classes-and-structs/using-properties)으로 넣어 설정할 수 있습니다.  

## 설명
특정 포맷으로 저장된 데이터를 Param으로 변환합니다.  
클래스를 Param으로 변환할 때는 위 파라미터의 주의사항을 참고해 주세요.  


## Example
```js
LitJson.JsonData json = new JsonData();
...  
Param param = Param.Parse(json);
```

## Exception case

**빈 데이터를 Param으로 변환 시도한 경우**  
예외 타입: ParseException  
예외 메시지: In order to convert JObject as Param, it must have child value.  

**자식이 존재하지 않는 json 데이터를 Param으로 변환 시도한 경우**  
예외 타입: ParseException  
예외 메시지: In order to convert JObject as Param, it must have child value.  
