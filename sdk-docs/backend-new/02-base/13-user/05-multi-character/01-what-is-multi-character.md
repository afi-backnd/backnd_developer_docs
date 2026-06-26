---
sidebar_label: 멀티 캐릭터란
description: "멀티 캐릭터란"
---

# 멀티 캐릭터란

:::caution

- 멀티 캐릭터 기능에 여러 가지 개선이 필요한 상황임을 확인하고, 현재는 멀티 캐릭터 프로젝트 생성을 제한한 상태입니다. 이미 멀티 캐릭터를 사용 중인 프로젝트에서는 계속 멀티 캐릭터 기능을 이용하실 수 있습니다.
- 만일 멀티 캐릭터 기능을 사용하지 않고 일반 프로젝트로 전환을 원하시는 경우, help@backnd.com로 문의해 주세요.
- 보다 편리하고 안정적으로 사용하실 수 있도록 정비 중이오니 너른 양해 부탁드립니다.

:::

## 개요

멀티 캐릭터란 한 계정 안에 여러개의 커스텀 로그인 유저를 가질 수 있는 기능입니다.  
RPG 장르에서 로그인 후, 캐릭터 선택화면에서 캐릭터 하나를 선택하여 로그인 하는 등의 게임을 만들 수 있습니다.

## 예제 프로젝트

멀티 캐릭터 로그인의 기초적인 사용법을 보여주는 [예제 프로젝트](/sdk-docs/backend/base/user/multi-character/unity-example-project)가 준비되어있습니다.  
해당 프로젝트를 통해 멀티 캐릭터 함수 사용 방법을 익힐 수 있습니다.

## 멀티 캐릭터 프로젝트 생성

멀티 캐릭터 기능을 이용하려면 프로젝트 생성 시, 멀티 캐릭터를 **사용**으로 설정해주셔야합니다.  
멀티 캐릭터가 **미사용**인 프로젝트에서 멀티 캐릭터를 생성할 경우, 멀티 캐릭터 전용 UI가 보이지 않아 이용에 불편할 수 있습니다.

<img src="https://developer.thebackend.io/static/img/unity/multiCharacter/info/project.png"/>

## 제한사항

멀티 캐릭터 기능 사용시 제한사항은 다음과 같습니다.

- 캐릭터의 총 생성 갯수 제한은 20개입니다.
- 푸시 알람은 제일 최근에 등록한 유저 한명에게만 발송이 됩니다.
- 내 캐릭터 리스트를 불러올 때 같이 불러올 수 있는 테이블은 1개입니다.
- 게정 로그인에서는 뒤끝 베이스의 호출이 불가능합니다.
- 따라서 소유중인 캐릭터들의 데이터를 총합하여 랭킹에 등록하거나 DB에 저장하는 식의 구현은 불가능합니다.
- 멀티 캐릭터의 아이디 혹은 비밀번호를 찾기 위한 수단은 SDK 5.11.1 버전부터 제공됩니다.
- 멀티 캐릭터 로그인의 경우, SDK 5.11.1 버전부터 GPGS, Siwa 등 서드파티 로그인이 지원됩니다.
- **원활한 멀티 캐릭터 기능 이용을 위해 SDK 5.11.1 이상 버전을 이용해주세요.**

## 멀티 캐릭터 로그인 프로세스

멀티 캐릭터의 경우, 로그인하기까지의 과정이 커스텀 로그인에 비해 추가되었습니다.  
<img src="https://developer.thebackend.io/static/img/unity/multiCharacter/info/multicharacter-process.png"/>

1. 멀티 캐릭터 계정 로그인
   먼저 커스텀 로그인과 동일하게 아이디와 비밀번호를 입력하여 멀티 캐릭터의 계정 로그인을 호출합니다.

2. 캐릭터 불러오기
   계정 로그인이 성공하면 해당 계정이 가지고 있는 모든 캐릭터의 리스트를 불러옵니다.  
   이때 캐릭터의 프로필 또는 스텟을 표시하기 위해 하나의 테이블을 지정하여 가져올 수 있습니다.

3. 불러온 정보로 캐릭터 로그인
   캐릭터에 로그인하려면 2에서 불러온 각 캐릭터의 uuid와 inDate가 필요합니다.

4. 로그인 이후 뒤끝 베이스 기능 이용
   캐릭터 선택(`SelectCharacter(uuid, inDate)`)까지 성공하였다면 로그인이 성공한 것입니다.  
   이후부터는 커스텀 로그인 성공 이후 로직과 동일하게 뒤끝 베이스 기능을 호출하는 것이 가능합니다.

## 멀티 캐릭터 로그인 이후 호출 불가한 함수

멀티 캐릭터로 로그인한 모든 캐릭터는 BackndAuth.Instance에 존재하는 다음 함수들을 사용할 수 없습니다.

- BackndAuth.Instance.UpdateCustomEmail
- BackndAuth.Instance.ResetPassword
- BackndAuth.Instance.ChangePassword
- BackndAuth.Instance.VerifyPassword
- BackndAuth.Instance.UpdateProviderEmail
- BackndAuth.Instance.LinkWithProvider
- BackndAuth.Instance.DeleteAccount

해당 함수를 호출할 경우 다음 에러가 리턴됩니다.

**캐릭터 로그인 이후 해당 함수를 호출한 경우**  
statusCode : 424  
errorCode : CharacterBlock  
message : Can not call by Character Login
