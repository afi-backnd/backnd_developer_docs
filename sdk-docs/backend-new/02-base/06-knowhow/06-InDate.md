# InDate

indate는 유저나 게임 정보, 랭킹, 길드, 우편 등 데이터가 생성될 때 해당 데이터의 Key 값으로 설정되는 값입니다.  
* **모든 유저, 테이블 각각의 row, 우편, 길드 등은 모두 각각의 inDate 값을 가집니다.**  
* **inDate는 데이터의 고유한 Key 값으로 사용**됩니다.(절대 중복되지 않습니다.)

## inDate 조회하는 방법
inDate는 데이터를 조회할 때 확인할 수 있습니다.  
inDate를 조회하는 다양한 방법은 아래 개발자 문서를 참고해주세요.  
* [RequestResult에서 inDate 조회 함수를 이용하여 inDate 값 확인](/sdk-docs/backend/base/knowhow/object-for-return/get-indate/get-indate-by-function)
* [RequestResult에서 수동으로 inDate 값 확인](/sdk-docs/backend/base/knowhow/object-for-return/get-indate/get-own-indate)
> 데이터를 조회하는 함수에는 게임 정보 조회, 우편 조회, 길드 조회 등 다양한 조회 함수가 포함됩니다.  

## 유저의 inDate
유저의 inDate는 절대 변하지 않고 고유한 유저의 Key 값 입니다.  
게임 정보 조회, 랭킹 조회, 우편 발송, 친구 추가 등의 기능에서 이를 이용하여 타인의 정보에 상호작용할 수 있습니다.  

유저의 inDate는 대표적으로 아래 방법을 이용하여 확인할 수 있습니다.  
또한 호출한 함수에 따라 다른 컬럼명 으로 리턴될 수 있습니다.  

| 방법 | inDate key 명 | 설명 |
| --- | --- | --- |
| [유저 자신의 inDate 값 확인](/sdk-docs/backend/base/user/get-my-information/get-from-local)| UserIndate | 로그인 한 유저의 inDate |
| [다른 유저의 inDate 값 확인](/sdk-docs/backend/base/find-user/find-by-nickname)| inDate | 검색한 유저의 inDate |
| [랭킹 리스트 조회](/sdk-docs/backend/base/rank/user/get-list)| gamerIndate | 해당 랭커의 inDate |
| [게임 정보 조회](/sdk-docs/backend/base/game-information/get-all/using-query) | owner_inDate | 해당 row를 소유한 유저의 inDate |
| [친구 리스트 조회](/sdk-docs/backend/base/friend/list-friends) | inDate | 친구의 inDate |
| [우편 리스트 조회](/sdk-docs/backend/base/post/difference-to-old#구버전-코드post) | receiverInDate | 수신인의 inDate |
| [우편 리스트 조회](/sdk-docs/backend/base/post/difference-to-old#구버전-코드post) | senderInDate | 발신인의 inDate |
| [길드 리스트 조회](/sdk-docs/backend/base/guild/search/get-all-guild) | masterInDate | 길드 마스터의 inDate |
| [길드 회원 리스트 조회](/sdk-docs/backend/base/guild/search/get-members-in-guild) | gamerInDate | 길드원의 inDate |

## 게임 정보에서의 inDate
뒤끝에서는 유저가 하나의 테이블에 여러 개의 row를 소유할 수 있습니다.  
이때 각각의 row를 식별하기 위해 각 row는 고유한 inDate를 key 값으로 가집니다.  
row의 inDate를 이용하여 해당 row를 조회, 수정, 삭제, 랭킹 갱신 할 수 있습니다.  

게임 정보에서의 inDate는 대표적으로 아래 방법을 이용하여 확인할 수 있습니다.  

| 방법 | inDate key 명 | 설명 |
| --- | --- | --- |
| [게임 정보 조회](/sdk-docs/backend/base/game-information/get-all/using-query) | inDate | row의 inDate |
| [내 게임 정보 조회](/sdk-docs/backend/base/game-information/get-own-game/using-query) | inDate | row의 inDate |

## 길드의 inDate
각 길드는 고유한 inDate를 Key 값으로 가집니다.  
길드의 inDate를 이용하여 해당 길드를 조회할 수 있습니다.  

길드의 inDate는 대표적으로 아래 방법을 이용하여 확인할 수 있습니다.  
또한 호출한 함수에 따라 다른 컬럼명 으로 리턴될 수 있습니다.  

| 방법 | inDate key 명 | 설명 |
| --- | --- | --- |
| [길드 리스트 조회](/sdk-docs/backend/base/guild/search/get-all-guild) | inDate | 길드의 inDate |
| [내 길드 정보 조회](/sdk-docs/backend/base/guild/search/get-my-guild) | inDate | 내 길드의 inDate |
| [길드명으로 길드 inDate 조회](/sdk-docs/backend/base/guild/search/get-guild-by-indate) | guildIndate | 조회한 길드의 inDate |

## 우편 / 쪽지의 inDate
각 우편 및 쪽지는 고유한 inDate를 Key 값으로 가집니다.  
해당 inDate를 사용하여 우편/쪽지 조회, 수령 및 삭제할 수 있습니다.  

우편 / 쪽지의 inDate는 아래 방법을 이용하여 확인할 수 있습니다.  

| 방법 | inDate key 명 | 설명 |
| --- | --- | --- |
| [우편 리스트 조회](/sdk-docs/backend/base/post/difference-to-old#구버전-코드post) | inDate | 우편의 inDate |
| [받은 쪽지 조회](/sdk-docs/backend/base/message/received/list-received) | inDate | 받은 쪽지의 inDate |
| [보낸 쪽지 조회](/sdk-docs/backend/base/message/sent/list-sent) | inDate | 보낸 쪽지의 inDate |
