# 뒤끝 비속어 필터링 SDK

## 설명
뒤끝 비속어 필터링 SDK는 로컬에 저장된 json 형식의 비속어 필터링 리스트를 불러와 비속어가 포함된 string인지 판별하는 기능을 제공하는 SDK입니다.

```js
TheBackend.ToolKit.InvalidFilter.FilterManager filterManager = new ();

// 비속어 필터링 SDK 초기화
if (filterManager.LoadInvalidString())
{
    Debug.Log("성공했습니다.");
}
else
{
    Debug.LogError("실패했습니다.");
}


string nickname = "시바이누";

if(filterManager.IsFilteredString(nickname)) {
    Debug.Log("비속어가 포함된 닉네임입니다");
    return;
}

// 닉네임 변경하기
Backend.BMember.UpdateNickname(nickname);
```

기본 동작은 다음과 같습니다.
1. Resources.Load를 통해 TheBackend > ToolKit > InvalidFilter > Resources 폴더에 존재하는 InvalidString 파일 불러오기
2. TextAsset 형식의 InvalidString을 Json으로 변환, JsonUtility를 통해 List로 변환
3. 뒤끝 필터링 검색 시스템에 List로 저장된 필터링 리스트 추가
4. GetFilteredString 함수를 통해 필터링된 문자열을 받거나 IsFilteredString를 통해 필터링이 감지된 문자열인지 판단

### SDK 다운로드
[BackendInvalidFilter-1.0.0.unitypackage](https://developer.thebackend.io/sdk/invalid-filter/BackendInvalidFilter-1.0.0.unitypackage) \[2024-03-13]  

### 비속어 필터링 리스트
비속어 필터링 리스트는 기본적으로 플러그인을 import 했을 때 다음 경로에 존재합니다. 

`TheBackend > ToolKit > InvalidFilter > Resources > InvalidString.json`

해당 파일을 수정하여 필터링 리스트를 수정할 수 있습니다.

파일을 아래와 같이 invalidString 라는 key에 배열 형태의 string 데이터가 존재해야합니다.
```js
{
    "invalidString" : [
        "10넘",
        "10놈",
        "10발"
    ]
}
```


## 함수

### 1. 비속어 필터링 클래스

비속어 필터링 기능을 사용하려면 먼저 TheBackend.ToolKit.InvalidFilter 네임 스페이스에 존재하는 FilterManager를 생성해야합니다.

```js
TheBackend.ToolKit.InvalidFilter.FilterManager filterManager = new ();
```

### 2-a. 비속어 필터링 리스트 불러오기(기본)
**public bool LoadInvalidString()**

기본 경로에 위치한 InvalidString 파일을 불러옵니다.  
정상적으로 불러왔을 경우, 해당 함수의 리턴값이 true, 실패할 경우 Debug.LogError 출력과 함께 false가 리턴됩니다.

> LoadInvalidString 함수에서는 Resource.Load("InvalidString")을 통해 json 파일을 불러옵니다.  
따라서 해당 파일에 대한 이름 변경 혹은 위치 변경이 이루어졌을 경우 해당 함수는 이용이 불가능하며 커스텀된 2-b 형식으로 이용하셔야합니다.  

```js
public void Initialize1()
{
    if (filterManager.LoadInvalidString())
    {
        Debug.Log("성공했습니다.");
    }
    else
    {
        Debug.LogError("실패했습니다.");
    }
}
```

### 2-b. 비속어 필터링 리스트 불러오기(커스텀)
**public bool LoadInvalidString(string jsonFileContents)**

기본 위치에 지정된 InvalidString.json의 이름을 변경하거나 위치를 변경하였을 경우, 직접 해당 json 파일을 로드하여 매게변수에 대입해야합니다.  

매개변수 **jsonFileContents**는 json 파일 위치가 아닌 json 파일을 string으로 변환한 값을 의미합니다.

```js
public void Initialize2()
{
    var textAsset = Resources.Load<TextAsset>("OtherFolder/InvalidString");
    if (textAsset == null)
    {
        Debug.LogError("해당파일이 존재하지 않습니다");
        return;
    }

    
    if(filterManager.LoadInvalidString(textAsset.text))
    {
        Debug.Log("성공했습니다.");
    }
    else
    {
        Debug.LogError("실패했습니다.");
    }
}
```

### 2-c. 비속어 필터링 추가하기
**public void AddFilterString(string word)**

뒤끝 필터링 리스트에 문자열을 추가합니다.  
만약 JsonUtility를 사용하지 못하거나, LoadInvalidString 함수에서 제공하는 양식을 이용하지 못하는 경우에 이용할 수 있습니다.

```js
public void Initialize3()
{
    var textAsset = Resources.Load<TextAsset>("InvalidString");

    LitJson.JsonData jsonData = JsonMapper.ToObject(textAsset.text);
    
    foreach (LitJson.JsonData word in jsonData["invalidString"])
    {
        filterManager.AddFilterString(word.ToString());
    }
}


public void Initialize4()
{
    filterManager.AddFilterString("개새");
    filterManager.AddFilterString("ㅅㅂ");
    filterManager.AddFilterString("뒤끝");
}
```


### 비속어 필터링 여부 확인하기
**public bool IsFilteredString(string filterCheckString)**

해당 문자열에 비속어가 포함되어있을 경우, true를 반환합니다.


```js

string nickname = "시바이누";

if(filterManager.IsFilteredString(nickname)) {
    Debug.Log("비속어가 포함된 닉네임입니다");
    return;
}

Backend.BMember.UpdateNickname(nickname);
```


### 비속어 필터링 문자로 치환하기
**public string GetFilteredString(string filterCheckString)**

문자열에 비속어가 포함되어 있을 경우, 해당 문자열 부분만 * 로 치환합니다.  
다른 문자로 치환하고 싶을 경우에는 **filterManager.SetFilterStringReplacementChar('문자');** 를 통해 치환 문자를 설정할 수 있습니다.  


```js
filterManager.SetFilterStringReplacementChar('뿅');

string otherUserIntroduction = filterManager.GetFilteredString("게임 ㅈㄴ 어렵네");
Debug.Log(otherUserIntroduction); // 게임 뿅뿅 어렵네

```

### 사용 예시
```js
using LitJson;
using UnityEngine;
using UnityEngine.UI;
using TheBackend.ToolKit.InvalidFilter;

public class NewBehaviourScript : MonoBehaviour
{

    private FilterManager filterManager = new FilterManager();

    void Start() 
    {
        Initialize2();
    }

    public void Initialize1()
    {
        if (filterManager.LoadInvalidString())
        {
            Debug.Log("성공했습니다.");
        }
        else
        {
            Debug.LogError("실패했습니다.");
        }
    }

    public void Initialize2()
    {
        var textAsset = Resources.Load<TextAsset>("InvalidString");
        if (textAsset == null)
        {
            Debug.LogError("해당파일이 존재하지 않습니다");
            return;
        }

        filterManager.LoadInvalidString(textAsset.text);
    }

    public void Initialize3()
    {
        var textAsset = Resources.Load<TextAsset>("InvalidString");

        LitJson.JsonData jsonData = JsonMapper.ToObject(textAsset.text);
        
        foreach (LitJson.JsonData word in jsonData["invalidString"])
        {
            filterManager.AddFilterString(word.ToString());
        }
    }
    
    public void Initialize4()
    {
        filterManager.AddFilterString("개새");
        filterManager.AddFilterString("ㅅㅂ");
        filterManager.AddFilterString("뒤끝");

    }

    public void CheckNickname()
    {
        string nickname = "제이름은 뒤   끝이에요";
        Debug.Log(filterManager.GetFilteredString(nickname)); // 결과값 : 제이름은 *   *이에요
        Debug.Log(filterManager.IsFilteredString(nickname)); // 결과값 : True
    }
}

```
