# Param

Param은 뒤끝 서버와 **통신을 할 때 넘겨주는 파라미터 클래스**입니다.  
뒤끝 서버와 상호작용하는 다양한 함수에서 Param을 인자 값으로 넘겨받습니다.  
Param은 다음과 같은 메소드를 제공합니다.  

| 메소드 | 리턴 값 | 설명  |
| :------------ |:------------| :-----|
| [Add](/sdk-docs/backend/base/knowhow/param/insert) | void | Param에 데이터를 추가합니다. |
| [AddCalculation](/sdk-docs/backend/base/knowhow/param/insert-calculated) | void | Param에 연산을 위한 데이터를 추가합니다. |
| [Parse](/sdk-docs/backend/base/knowhow/param/transfer-to-param) | Param | json 형태의 데이터 혹은 클래스를 Param으로 변환합니다. |
| Clear | void | Param에 추가되어 있는 데이터를 초기화합니다. |
| Clone | Param | Param을 복사합니다. |
| GetJson | string | Param에 저장된 데이터를 Json string 형태로 리턴합니다. |
| GetValue | SortedList | Param에 저장된 SortedList를 리턴합니다. |
> **Add, AccCalculation, Parse** 함수에 대한 자세한 설명은 해당 함수의 개발자 문서를 참고해 주세요.  

## Clear()
Param에 저장된 데이터를 초기화합니다.  

```csharp
Clear() -> void

// example
Param param = new Param();
...  
param.clear();
```

## Clone()
Param을 복사합니다.  

```csharp
Clone() -> Param

// example
Param copyParam = param.Clone();
```

## GetJson()
Param에 저장된 데이터를 Json 형태의 String으로 리턴합니다.  

```csharp
GetJson() -> string

// example
Param param = new Param();
...  
var json = param.GetJson();
```

## GetValue()
Param에 저장된 데이터를 리턴합니다.  

```csharp
GetValue() -> SortedList

// example
Param param = new Param();
...  
var sList = param.GetValue();
```

