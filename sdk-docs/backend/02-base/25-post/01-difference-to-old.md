---
sidebar_label: "구버전 우편 기능과의 차이점"
description: "구버전 우편 기능과의 차이점"
---

# 구버전 우편 기능과의 차이점

## 우편 불러오기 로직의 개편

이전 우편 기능(**Social.Post**)의 불러오기 기능은 우편함에 있는 전체 우편 중 제일 최신에 발송된 우편 100개를 읽어오는 로직이었습니다.  
해당 로직으로 대부분의 우편은 불러올 수 있었으나 유효한 100번째 이상의 우편은 불러올 수 없었습니다.  

이러한 상황을 개선하고자 우편 기능이 새롭게 개선되었습니다.  
새로운 우편 기능(**UPost**)의 불러오기 기능은 자신이 받을 수 있는 제일 오래된 우편부터 limit의 수만큼 불러오기 시작합니다.  
이후 호출할 때마다 마지막으로 불러온 우편에서 시작하여 점차 최신 날짜의 우편을 불러오게 됩니다.  
이로 인해 첫 호출에는 우편을 불러오지 못해도 호출할수록 누적되어 우편 리스트가 갱신하게 됩니다.  

limit은 10이 기본으로 수가 작을수록 빠르며, 수가 높을수록 느리지만 많은 양의 우편을 불러옵니다.  

만약 이전 우편 기능처럼 이용하고 싶으시다면 limit의 값을 100으로 지정하여 함수를 호출하시기 바랍니다.  

> Backend.UPost.GetPostList(PostType.Admin, 100);

## 콘솔 우편 기능의 추가

2021년 12월 28일 콘솔의 리뉴얼과 함께 우편 기능에 다음과 같은 기능이 추가되었습니다.  

1. 아이템 종류의 개수 1개 -> 0~ 10개까지 선택 가능
2. 우편의 저자(글쓴이) 입력 가능
3. 보내는 사람 차트 업로드 기능

위의 추가된 기능들 중 SDK에서 기존 버전의 우편 기능들은 1번과 2번 기능들을 사용할 수 없습니다.  
해당 기능들은 새로운 우편 기능에서 사용하실 수 있습니다.  

> 기존 우편 기능의 경우 아이템이 1개인 우편에 관해서만 불러오기 및 수령이 가능합니다.  

## SDK 우편 기능의 차이

### 구버전 우편 기능

구버전에는 다음과 같은 차이점이 있습니다.  

1.  콘솔에서 보낸 우편(관리자 + 우편)과 유저가 보낸 우편을 우편 불러오기 함수를 통해 한 번에 불러올 수 있습니다.  
2.  우편 불러오기와 우편 수령 시, 리턴되는 Json에 데이터 타입 "M", "N", "L" 등이 추가로 리턴됩니다.  
3.  관리자 우편의 아이템 종류의 개수가 1개인 우편만 불러올 수 있습니다.(그 외에 우편은 리스트에서 제외됩니다)
4.  author(저자, 보내는 이)를 입력할 수 없습니다.  
5.  전체 우편중 최신 100개의 우편 리스트를 불러옵니다.  

### 신버전 우편 기능

신버전에는 다음과 같은 차이점이 있습니다.  

1. 관리자, 랭킹, 유저 중 하나를 선택하여 해당 종류의 우편을 불러옵니다.  
2. 우편 불러오기, 우편 수령 시 리턴되는 Json에 데이터 타입 "M", "S", "L" 형식이 제거되었습니다.  
3. 관리자 우편에서 아이템 종류의 개수 상관없이 모든 우편을 받아올 수 있습니다.  
4. author 저자를 입력할 수 있습니다.  
5. 오래된 우편 리스트로부터 limit의 수만큼 검색하여 리스트를 갱신합니다.  

## 변경된 우편 기능의 코드

신버전과 구버전 우편 리턴되는 값이 다소 변경되었습니다.  

### 신버전 코드(UPost)

```js
public void GetPostList()
{
    var bro = Backend.UPost.GetPostList(PostType.Admin, 100);
    LitJson.JsonData json = bro.GetReturnValuetoJSON()["postList"];

    for(int i = 0; i < json.Count; i++)
    {
          string postTitle = json[i]["title"].ToString(); // 우편 제목
          string postContent = json[i]["content"].ToString(); // 우편 내용
          string  postAuthor = json[i]["author"].ToString(); // 우편 저자

          int postItemCount = (int)json[i]["itemCount"]; // 문제가 되는 데이터타입 제거로 인해 코드 간편화

          string postIndate = json[i]["inDate"].ToString();

          ReceivePostItem(PostType.Admin, postIndate); // 우편 수령
    }
}
public void ReceivePostItem(PostType postType, postIndate)
{
    var bro = Backend.UPost.ReceivePostItem(postType, postIndate);
    LitJson.JsonData postList = bro.GetReturnValuetoJSON()["postItems"];
    for(int i=0; i < postList.Count; i++)
    {
         if(postList[i].Count <= 0)
         {
              Debug.Log("아이템이 없는 우편");
             continue;
         }
        string itemName = postList[i]["item"]["itemName"];
        int itemCount = (int)postList[i]["itemCount"];
    }
}
```

### 구버전 코드(Post)

```js
var bro = Backend.Social.Post.GetPostListV2();
LitJson.JsonData json = bro.GetReturnValuetoJSON()["fromAdmin"];

for(int i = 0; i < json.Count; i++)
{
      string postTitle = json[i]["title"]["S"].ToString();
      string postContent = json[i]["content"]["S"].ToString();
      int postItemCount = string.Empty;
      if(json[i]["itemCount"].ContainsKey("S")) // 랭킹 우편의 경우 모든 데이터 타입이 S로 나와서 예외 처리를 해야합니다.  
      {
          postItemCount = (int)json[i]["itemCount"]["S"];
      }
      if(json[i]["itemCount"].ContainsKey("N")) // 랭킹 우편과  관리자 우편의 itemCount 데이터 타입이 다릅니다.  
      {
          postItemCount = (int)json[i]["itemCount"]["N"];
      }
      string postIndate = json[i]["inDate"]["S"].ToString();

}

public void ReceivePostItem()
{
      var bro = Backend.Social.Post.ReceiveAdminPostItemV2(postIndate);

     LitJson.JsonData json = bro.GetReturnValuetoJSON();

      string itemName = json[i]["item"]["M"]["itemName"]["S"].ToString();
      int itemCount = (int)json["itemCount"]["S"];
    }
```
