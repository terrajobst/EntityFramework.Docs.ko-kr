---
title: 새로운 기능 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: fcd6339f67a1512dd66220c59537d12cf0b22620
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490300"
---
# <a name="whats-new-in-ef6"></a>EF6의 새로운 기능

최신 기능 및 높은 안정성을 얻을 수 있도록 Entity Framework 최신 릴리스 버전을 사용하는 것이 좋습니다.
하지만 이전 버전을 사용해야 하거나 최신 시험판 릴리스의 개선된 기능을 사용해 보려는 분들도 있다는 사실을 잘 알고 있습니다.
EF 특정 버전을 설치하는 방법은 [Entity Framework 받기](~/ef6/fundamentals/install.md)를 참조하세요.

이 페이지에서는 각 새 릴리스에 포함된 기능을 설명합니다.

## <a name="recent-releases"></a>최신 릴리스

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Visual Studio 2017 15.7의 EF 도구 업데이트

2018년 5월에 Visual Studio 2017 15.7의 일부로 업데이트된 버전의 EF 도구가 릴리스되었습니다.
공통적인 몇 가지 문제점이 이 버전에서 개선되었습니다.

- 여러 사용자 인터페이스 접근성 버그 수정
- 기존 데이터베이스에서 모델을 생성하는 경우 SQL Server 성능 회귀에 대한 해결 [#4](https://github.com/aspnet/entityframework6/issues/4)
- SQL Server의 대규모 모델에 모델을 업데이트하는 지원 [#185](https://github.com/aspnet/EntityFramework6/issues/185)

또한 새 버전의 EF 도구는 새 프로젝트에서 모델을 만들 때 EF 6.2 런타임을 설치하도록 개선되었습니다. 이전 버전의 Visual Studio를 사용하는 경우 해당하는 NuGet 패키지 버전을 설치하여 EF 6.2 런타임(그리고 이전 버전의 EF)을 사용할 수 있습니다.

### <a name="ef-62-runtime"></a>EF 6.2 런타임

EF 6.2 런타임은 2017년 10월에 NuGet에 출시되었습니다.
오픈 소스 기여자 커뮤니티의 적극적인 참여 덕분에 EF 6.2는 여러 [버그 픽스](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) 및 [향상된 제품 기능](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20)을 포함하고 있습니다.

다음은 EF 6.2 런타임에 영향을 주는 가장 중요한 변경 내용을 간략하게 정리한 목록입니다.

- 영구 캐시에서 완료된 Code First 모델을 로드하여 시작 시간 단축 [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- 인덱스를 정의할 수 있는 흐름 API [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- SQL에서 LIKE로 해석되는 LINQ 쿼리를 쓸 수 있게 해주는 DbFunctions.Like() [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- 이제 Migrate.exe가 -script 옵션 지원 [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- 이제 EF6는 SQL Server에서 시퀀스를 통해 생성된 키 값 사용 가능 [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- SQL Azure 실행 전략에 대한 일시적인 오류 목록 업데이트 [83](https://github.com/aspnet/EntityFramework6/issues/83)
- 버그: "SqlParameter가 이미 다른 SqlParameterCollection에 포함되어 있음" 메시지와 함께 쿼리 재시도 또는 SQL 명령이 실패 [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- 버그: 디버거에서 DbQuery.ToString() 평가가 자주 시간 초과 [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="future-releases"></a>이후 릴리스

EF6 이후 버전에 대한 내용은 [로드맵](roadmap.md)을 참조하세요.

## <a name="past-releases"></a>이전 릴리스

[이전 릴리스](past-releases.md) 페이지에는 모든 EF 이전 버전 및 각 릴리스에 도입된 주요 기능 아카이브가 포함되어 있습니다.
