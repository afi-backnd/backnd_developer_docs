---
title: Backend-5.18.7
date: 2026-02-24T10:00
slug: backend-5-18-7
---

:::danger 5.18.7 지원 종료
신규 버전 5.18.8가 릴리즈 됨에 따라 5.18.7 버전의 지원이 종료되었습니다.  
최신 버전으로 업데이트 해 주시기 바랍니다.
:::

:::info 업데이트 요약
[게임 운영 관리] 공지사항, 이벤트, 약관 및 정책 기능이 개선되었습니다.  
[쿠폰 관리] Web Coupon 을 직접 사용할 수 있는 기능이 추가되었습니다.  
[Deprecated] 랭킹, 차트, 확률, 약관 및 정책, 길드 구버전 함수들이 삭제되었습니다.
:::

<!--truncate-->

[SDK .NET 4 버전] <a href="https://developer.thebackend.io/sdk/unityPackage/5.18.7/Backend-5.18.7.unitypackage" target="_blank">다운로드</a>   

## Versions
- Backend-5.18.7.dll
- Backend-1.1.0.aar

## 5.18.7 Update
**[Updates]**
- [게임 운영 관리] 공지사항, 이벤트, 약관 및 정책 기능이 개선되었습니다.
   - 공지사항, 이벤트, 약관 및 정책 기능이 뒤끝 콘솔에서 언어 별로 관리할 수 있도록 개선되었습니다.
- [쿠폰 관리] Web Coupon 을 직접 사용할 수 있는 기능이 추가되었습니다.
   - Web Coupon을 직접 사용할 수 있는 기능이 추가되었습니다. 웹뷰, SDK 함수 등 다양한 방식으로 Web Coupon을 사용할 수 있습니다.

**[Deprecated] 신규 함수들이 제공됨에 따라 Deprecated 되었던 함수들이 삭제되었습니다.**
- [랭킹]
    - Backend.URank.User.GetRankTableList
    - Backend.URank.User.GetRankList
    - Backend.URank.User.GetMyRank
    - Backend.URank.User.GetUserRank
    - Backend.URank.User.GetRankListByScore
    - Backend.URank.User.GetRankRewardList
    - Backend.URank.User.GetPastRankList
    - Backend.URank.User.UpdateUserScore
    - Backend.URank.Guild.GetRankTableList
    - Backend.URank.Guild.GetRankList
    - Backend.URank.Guild.GetMyGuildRank
    - Backend.URank.Guild.GetGuildRank
    - Backend.URank.Guild.GetRankListByScore
    - Backend.URank.Guild.UpdateGuildMetaData
    - Backend.URank.Guild.ContributeGuildGoods
    - Backend.URank.Guild.UseGuildGoods
    - Backend.URank.Guild.GetRankRewardList
    - Backend.URank.Guild.GetPastRankList
- [차트]
    - Backend.Chart.GetChartList
    - Backend.Chart.GetChartListV2
    - Backend.Chart.GetChartListByFolder
    - Backend.Chart.GetChartListByFolderV2
    - Backend.Chart.GetChartContents
    - Backend.Chart.GetOneChartAndSave
    - Backend.Chart.GetOneChartAndSaveV2
    - Backend.Chart.GetAllChartAndSave
    - Backend.Chart.GetAllChartAndSaveV2
    - Backend.Chart.GetChartByFolderAndSave
    - Backend.Chart.GetChartByFolderAndSaveV2
    - Backend.Chart.GetLocalChartData
    - Backend.Chart.GetLocalChartDataV2
    - Backend.Chart.DeleteLocalChartData
    - Backend.Chart.DeleteLocalChartDataV2
- [확률]
    - Backend.Probability.GetProbabilityContents
    - Backend.Probability.GetProbabilityCardList
    - Backend.Probability.GetProbabilityCardListV2
- [약관 및 정책]
   - Backend.Policy.GetPolicy
- [길드]
    - Backend.Guild.UseGoodsV3
    - Backend.Guild.UseGoodsV4
    - Backend.Guild.ModifyGuildV3
    - Backend.Guild.ModifyGuildV4
    - Backend.Guild.ContributeGoodsV3
    - Backend.Guild.ContributeGoodsV4

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
