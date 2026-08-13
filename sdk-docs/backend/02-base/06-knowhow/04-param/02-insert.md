---
sidebar_label: "데이터 삽입"
description: "Add"
---

# Add
public void **Add**(string **key**, T **value**);

## 파라미터

| Value|  Type | Description |
| --- | --- | --|
| key| string | Add할 컬럼의 key 값입니다.  |
| value | T | Add할 컬럼의 value 값입니다.  |

## 설명
Param에 컬럼을 추가합니다.  
* **Key는 숫자로 시작할 수 없습니다.** 숫자로 시작하는 경우, 경고가 출력되며 Param에 추가되지 않습니다.  
* **Value에는 c#에서 지원하는 값 형식을 모두 추가할 수 있습니다.**  
* SDK 5.2.1 미만 버전에서는 Param에 Add 할 때 데이터 타입의 제약이 있습니다.  
* 5.2.1 미만 버전에서 Add 할 수 있는 데이터 타입은 문서 제일 하단의 표를 참고해 주세요.  
* float/double형 데이터의 경우 반올림이 될 수 있습니다.  
* Dictionary 형태의 데이터의 경우 **3depth를 초과하는 경우 에러가 발생할 수 있습니다.**  
* 클래스 형식의 데이터를 포함한 경우 클래스 내 Private 변수는 Param에 추가되지 않습니다.  
> private 변수 중 param에 추가하고 싶은 값은 [property(속성)](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/classes-and-structs/using-properties)으로 넣어 설정할 수 있습니다.  

## Example

```js
Param param = new Param();
param.Add("hp", 512);
```


## SDK 5.2.1 미만 버전에서 Add 할 수 있는 데이터 타입

| 데이터 타입 | 설명 |
| :------------ |:-------------|
| int | 부호 있는 32비트 정수 |
| long | 부호 있는 64비트 정수 |
| double | 부호 있는 8바이트 부동 소수점.  |
| bool | 부울(true/false) |
| string | 0자 이상의 유니코드 문자 시퀀스 |
| Param | 뒤끝 제공 Param |
| JsonData | LitJson.JsonData 타입으로 저장된 json 데이터  
해당 자료형은 sdk-5.0.1 이상 버전 부터 지원합니다. |
| T | **레퍼런스 타입 제너릭**. 개발자가 선언한 임의의 클래스, 위 자료형에 포함되지 않는 데이터가 모두 포함됩니다. |
| Array | 배열 형태의 데이터 |
| List | 리스트 형태의 데이터 |
| Dictionary<string, T> | 딕셔너리 형태의 데이터 |
