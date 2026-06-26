---
sidebar_label: 함수로 조회
description: "함수로 조회"
---

# GetGoogleHash

public string **GetGoogleHash**();

## 설명

빌드 된 안드로이드 apk에서 구글 해시 키를 받아오는 기능입니다.  
이는 안드로이드 앱과 뒤끝 서버 사이의 정보교환 인증을 위해 필수적인 정보입니다.  
**안드로이드 기기에 빌드 될 때만 해시키**를 반환하며, 이를 뒤끝 콘솔에 등록해 주시기 바랍니다.  

## Example

```js
BackndUtils.Instance.GetGoogleHash();

//example
string googlehash = BackndUtils.Instance.GetGoogleHash();

Debug.Log("구글 해시 키 : " + googlehash);
```

## ReturnCase

### Success cases

**해시키 받아오기에 성공한 경우**  

```js
//example
9DVI3dgk2gjdjjlh+=
```

## 참고 영상

  

<div >
    <iframe width="560" height="315" src="https://www.youtube.com/embed/44k24YoDC-k?vq?=hd1080&rel=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen ></iframe>
</div>
