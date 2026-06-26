---
sidebar_label: 우편, 공지 설정 방법
description: "우편, 공지 설정 방법"
sidebar_position: 8
---

# 우편 설정 방법

:::tip 우편에 아이템을 포함시키고 싶은 경우
우편에 아이템을 포함하여 발송하고자 할 경우, 보낼 아이템이 존재하는 차트에서 우편 기능 여부가 사용인 경우에만 아이템을 포함하여 발송이 가능합니다.  
Foodtruck Venture에서 우편으로 발송 가능한 아이템이 포함된 차트는 'RewardItemData' 입니다.  
:::

### 우편 생성

콘솔 우편 메뉴에서 우편 등록을 클릭하면 보낼 우편에 대한 정보를 설정 할 수 있습니다.  
발송일, 만료기간, 제목, 내용 등을 입력합니다.

![image](/img/docs/guide/base/example-games/idle-tycoon/createPost.png)

우편에 아이템을 포함하여 발송하고 싶은 경우 아이템 등록을 클릭해 보낼 아이템을 선택합니다.  
보낼 수 있는 아이템은 차트에서 우편 사용 설정으로 되어있는 차트의 내용만 아이템 등록이 가능합니다.  

![image](/img/docs/guide/base/example-games/idle-tycoon/addItemPost.png)

우편 발송 대상을 선택합니다. 전체 유저에게 보내거나 특정 유저에게만 우편을 보낼 수 있습니다.  
발송 대상을 지정 하기 위해서는 직접 유저 정보를 입력하거나 유저 번호나 닉네임이 입력된 xlsx, csv파일을 업로드하여 보냅니다.  
아래 이미지는 유저 닉네임을 입력하여 발송 대상을 지정하였습니다.  

![image](/img/docs/guide/base/example-games/idle-tycoon/userFindPost.png)


모든 정보를 입력하고 확인 버튼을 클릭하면 우편이 발송됩니다.  
우편 발송 즉시 게임 내에서 우편을 확인 할 수 있습니다.  

![image](/img/docs/guide/base/example-games/idle-tycoon/completePost.png)


뒤끝 콘솔에서 우편 리스트를 불러와 SO_MailData 스크립터블 오브젝트에 저장하게 됩니다.  
  
우편 시스템이 구현된 부분은 아래의 파일을 열어 확인 하시기 바랍니다.  
- BakcndMail.cs
- AbstractMailReceiveReward.cs  

우편 기능의 자세한 내용은 [우편 기능](/sdk-docs/backend/base/post/kinds)에서 확인 할 수 있습니다.  

# 공지 생성 방법

### 공지 생성 

콘솔 공지 메뉴에서 게임 실행시 나타나는 공지를 설정 할 수 있습니다.  
게시 일시를 설정하여 즉시 또는 예약 게시를 선택하고 공지의 제목과 내용을 입력합니다.  

![image](/img/docs/guide/base/example-games/idle-tycoon/createNotice.png)

이미지를 첨부하면 공지에 이미지가 삽입됩니다.  
버튼 이름과 URL 생성 칸에 값을 입력하면 공지에 버튼이 생성되며 버튼을 클릭하면 해당 URL로 이동합니다.  

![image](/img/docs/guide/base/example-games/idle-tycoon/addtionalOptionNotice.png)

공지 시스템이 구현된 부분은 아래의 파일을 열어 확인 하시기 바랍니다.  
- BackndAnnouncement.cs
- AbstractMailReceiveReward.cs  

공지 기능의 자세한 내용은 [공지사항](/sdk-docs/backend/base/operation/notice/get-list)에서 확인 할 수 있습니다.  
