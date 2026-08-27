---
description: "게임 운영 관리"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 게임 운영 관리
뒤끝에서는 게임 운영에 필요한 다양한 기능들을 제공합니다.  

## 공지사항
게임 운영에 필요한 공지사항을 등록하고 관리할 수 있습니다.  
- 등록 형태는 즉시 게시 및 예약 게시 형태로 제공되며, 예약 게시는 30분 단위로 가능토록 제공됩니다.  
- 유저의 국가 코드에 따라 국가별로 다른 내용(다른 언어)으로 공지사항을 등록할 수 있습니다.  
- 이미지 첨부 및 URL 링크 버튼 생성 기능을 활용할 수 있습니다.  
- 일반 공지사항 외 로그인 없이도 조회가 가능한 임시 공지사항을 등록하고 관리할 수 있습니다.  

<ConsoleLinkButton text="공지사항 바로가기" menu="baseNotice" feature="공지사항" title="게임 운영 관리" />

관련 콘솔 가이드 : [뒤끝베이스 - 공지사항](/guide/console-guide/backnd-base/notice/notice)  
관련 SDK 개발자 문서 : [운영 관리 - 공지사항 조회](/sdk-docs/backend/base/operation/notice/get-list)


## 이벤트
게임 운영에 필요한 이벤트를 등록하고 관리할 수 있습니다.  
- 등록 형태는 즉시 게시 및 예약 게시 형태로 제공되며, 예약 게시는 30분 단위로 가능토록 제공됩니다.  
- 이벤트의 진행 기간은 총 4가지(하루, 일주일, 30일, 직접 설정)로 선택해 설정할 수 있습니다.  
- 유저의 국가 코드에 따라 국가별로 다른 내용(다른 언어)으로 공지사항을 등록할 수 있습니다.  
- 팝업 이미지, 콘텐츠 이미지 첨부 및 URL 링크 버튼 생성 기능을 활용할 수 있습니다.  

<ConsoleLinkButton text="이벤트 바로가기" menu="baseEvent" feature="이벤트" title="게임 운영 관리" />

관련 콘솔 가이드 : [뒤끝베이스 - 이벤트](/guide/console-guide/backnd-base/event/event)  
관련 SDK 개발자 문서 : [운영 관리 - 이벤트 조회](/sdk-docs/backend/base/operation/event/get-list)

## 약관 및 정책
서비스 이용약관 및 개인 정보 처리 방침을 등록 게시할 수 있습니다.  
등록한 정책은 SDK를 통해 문자열 형태와 URL 형태로 불러올 수 있습니다.  

<ConsoleLinkButton text="약관 및 정책 바로가기" menu="basePolicy" feature="약관 및 정책" title="게임 운영 관리" />

관련 콘솔 가이드 : [뒤끝베이스 - 약관 및 정책](/guide/console-guide/backnd-base/policy)  
관련 SDK 개발자 문서 : [운영 관리 - 운영 정책 조회](/sdk-docs/backend/base/operation/get-policy)

## 1:1 문의
고객과의 커뮤니케이션을 위한 1:1 문의 기능을 제공합니다.  
- 1:1 문의는 웹뷰 및 함수(유니티의 UGUI) 형태로 제공됩니다.  
- 개발사와 고객의 편의를 위해 문의 양식을 설정할 수 있습니다.  
- 고객의 원활한 문의를 위해 파일 첨부 기능이 제공됩니다.(웹뷰 방식에만 제공)  

<ConsoleLinkButton text="1:1 문의 바로가기" menu="baseInquiry" feature="1:1 문의" title="게임 운영 관리" />

관련 콘솔 가이드 : [뒤끝베이스 - 1:1 문의](/guide/console-guide/backnd-base/inquiry)  
관련 SDK 개발자 문서 : [운영 관리 - 1대1 문의](/sdk-docs/backend/base/operation/question/using-webview/one-on-one)

## 유저 정보 찾기
이용자가 본인의 커스텀 계정의 아이디 및 비밀번호를 손쉽게 확인할 수 있는 기능을 제공합니다.  
- 뒤끝 콘솔을 통해 이용자의 이메일로 전송하는데 필요한 템플릿을 작성할 수 있습니다.  
- 이용자의 계정이 정보 찾기용 이메일이 등록되어 있어야 합니다.  
- 여러 국가에 서비스가 이루어지는 경우 템플릿 또한 각 국가 코드에 맞춰 작성할 실 수 있도록 제공합니다.  

<ConsoleLinkButton text="유저 정보 찾기 바로가기" menu="baseSearchGamer" feature="유저 정보 찾기" title="게임 운영 관리" />

관련 콘솔 가이드 : [뒤끝베이스 - 유저 정보 찾기](/guide/console-guide/backnd-base/search-user)  
관련 SDK 개발자 문서 : [유저 관리 - 아이디 찾기](/sdk-docs/backend/base/user/custom/find-custom-id)

## 쿠폰
서버에서 쿠폰 코드 발급 및 인증 서비스를 제공하며,  
이용자들의 쿠폰 사용 내역 또한 효율적으로 관리할 수 있습니다.  
- 하나의 코드로 다수의 이용자가 사용 가능한 단일 코드 쿠폰
- 각기 다른 코드로 발급 가능한 대규모의 시리얼 쿠폰
- 다수의 쿠폰 관리를 효율적으로 할 수 있도록 Excel 문서화 기능 제공합니다.  

<ConsoleLinkButton text="쿠폰 바로가기" menu="baseCoupon" feature="쿠폰" title="게임 운영 관리" />

관련 콘솔 가이드 : [뒤끝베이스 - 쿠폰 관리](/guide/console-guide/backnd-base/coupon/basic)  
관련 SDK 개발자 문서 : [쿠폰 관리 - 쿠폰 기능](/sdk-docs/backend/base/coupon/get-list)

## 푸시 알림
푸시 알람을 등록하고 관리할 수 있습니다.  
- Android 플랫폼과 iOS 플랫폼을 각각 선택 발송할 수 있습니다.  
- 등록 형태는 즉시 발송 및 예약 발송 형태로 제공되며, 
예약 발송은 30분 단위로 최대 30일 후까지 가능합니다.  
- 일간, 주간, 월간으로 원하는 시간대에 푸시를 반복적으로 발송할 수 있습니다.  
- 유저의 국가 코드에 따라 국가별로 다른 내용(다른 언어)으로 공지사항을 등록하여 제공할 수 있습니다.  

<ConsoleLinkButton text="푸시 알림 바로가기" menu="basePush" feature="푸시 알림" title="게임 운영 관리" />

관련 콘솔 가이드 : [뒤끝베이스 - 푸시 알림](/guide/console-guide/backnd-base/push)  
관련 SDK 개발자 문서 : [푸시 설정 - 푸시 등록](/sdk-docs/backend/base/push/android/active)
