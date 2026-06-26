---
sidebar_label: 언마샬(데이터 자료형 지우기)
description: "언마샬(데이터 자료형 지우기)"
---

# GetJObject, GetJArray, GetRows

## 설명
GetJObject(), GetJArray(), GetRows() 함수를 호출하면 [데이터 타입](/sdk-docs/backend/base/knowhow/object-for-return/data-structure)을 지우고 **순수하게 저장된 데이터만 추출**한 Json 오브젝트를 전달받게 됩니다.
* 데이터 언마샬을 통해 저장된 데이터를 사용하기 편리하게 변경할 수 있습니다.  
* 숫자형 데이터 중 string으로 저장되어 있는 데이터들을 각각 자료형에 맞게 long 혹은 double로 변환합니다.  
* 언마샬한 데이터는 클래스로 디시리얼라이즈 할 수 있습니다. [디시리얼라이즈 문서](/sdk-docs/backend/base/knowhow/object-for-return/deserialize)를 참고해 주세요. 
* 해당 함수를 수행할 때 1차적으로 JObject형태로 만든 후, 2차로 전체를 순회하며 데이터 타입을 제거하게 됩니다.  
**일반적인 경우 속도의 문제가 없지만 데이터가 복잡하고 큰 경우 퍼포먼스 이슈가 생길 수 있습니다.**  
> 아래 ReturnValue와 동일한 데이터 100개를 포함한 GetRows()를 언마셜 하는 데 **약 11ms**가 소요되었습니다.  
* 해당 함수를 호출하지 않으면 JObject 생성 및 언마셜을 진행하지 않습니다. 원본 문자열로 직접 처리하시려면 ReturnValue를 사용하세요.  

## Example

```js
var reqResult = await BackndUserData.Instance.GetDataAsync("inventory", 100);
if (reqResult.IsSuccess() == false)
{
    return;
}

// 서버의 응답을 json 객체로 변환 및 자동 언마셜링
var json = reqResult.GetJObject();
```

## ReturnValue

**언마샬 한 후 리턴 값(데이터 타입을 모두 삭제)**  
```js
{
    {
    "client_date": "2020-11-02T08:11:04.609Z",
    "nickname": "애플망고",
    "def": 75,
    "inDate": "2020-11-02T08:11:04.79Z",
    "atk": 63,
    "option": {
      "opt1": "empty",
      "opt2": "empty"
    },
    "updatedAt": "2020-11-02T08:11:04.786Z",
    "socket": [
      115,
      174,
      257
    ],
    "name": "name39",
    "critical": 2.71338716834476
  }
}
```

**언마샬 하기 전 리턴 값(데이터 타입을 포함)**  
```js
{
    {
    "client_date": {
      "S": "2020-11-02T08:11:05.631Z"
    },
    "nickname": {
      "S": "애플망고"
    },
    "def": {
      "N": "42"
    },
    "inDate": {
      "S": "2020-11-02T08:11:05.828Z"
    },
    "atk": {
      "N": "55"
    },
    "option": {
      "M": {
        "opt1": {
          "S": "empty"
        },
        "opt2": {
          "S": "empty"
        }
      }
    },
    "updatedAt": {
      "S": "2020-11-02T08:11:05.826Z"
    },
    "socket": {
      "L": [
        {
          "N": "115"
        },
        {
          "N": "170"
        },
        {
          "N": "255"
        }
      ]
    },
    "name": {
      "S": "name47"
    },
    "critical": {
      "N": "2.81677461593262"
    }
  }
}
``` 
