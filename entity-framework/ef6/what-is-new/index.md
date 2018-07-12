---
title: 새로운 기능 - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
caps.latest.revision: 3
ms.openlocfilehash: 0da2ce778a765037ecacd0726cbb7cda08b5683f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911707"
---
# <a name="whats-new-in-ef6"></a>EF6의 새로운 기능

최신 기능 및 높은 안정성을 얻을 수 있도록 Entity Framework 최신 릴리스 버전을 사용하는 것이 좋습니다.
하지만 이전 버전을 사용해야 하거나 최신 시험판 릴리스의 개선된 기능을 사용해 보려는 분들도 있다는 사실을 잘 알고 있습니다.
EF 특정 버전을 설치하는 방법은 [Entity Framework 받기](~/ef6/fundamentals/install.md)를 참조하세요.

이 페이지에서는 각 새 릴리스에 포함된 기능을 설명합니다.

## <a name="recent-releases"></a>최신 릴리스

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Visual Studio 2017 15.7의 EF 도구 업데이트

2018년 5월에 Visual Studio 2017 15.7의 업데이트된 EF6 도구 버전이 출시되었습니다.
공통적인 불편 사항 몇 가지가 이 버전에서 개선되었습니다.

- 사용자 인터페이스의 접근성 점검
- 리버스 엔지니어링에 대한 SQL Server 성능 저하 문제 해결 [#4](https://github.com/aspnet/entityframework6/issues/4)
- SQL Server의 더 큰 모델을 위해 데이터베이스 지원에서 모델 업데이트 [#185](https://github.com/aspnet/EntityFramework6/issues/185)

새 버전의 EF 도구는 새 프로젝트에서 모델을 만들 때 EF 6.2 런타임을 설치합니다. 이전 버전의 Visual Studio를 사용하는 경우 해당하는 NuGet 패키지 버전을 설치하여 EF 6.2 런타임(그리고 이전 버전의 EF)을 사용할 수 있습니다.

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
