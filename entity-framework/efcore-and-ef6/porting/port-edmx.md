---
title: EF6에서 EF Core로 포팅 EDMX 기반 모델 포팅-EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182057"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>EF Core EF6 EDMX 기반 모델 포팅

EF Core는 모델에 대 한 EDMX 파일 형식을 지원 하지 않습니다. 이러한 모델을 이식 하는 가장 좋은 옵션은 응용 프로그램의 데이터베이스에서 새 코드 기반 모델을 생성 하는 것입니다.

## <a name="install-ef-core-nuget-packages"></a>NuGet 패키지 EF Core 설치

`Microsoft.EntityFrameworkCore.Tools` NuGet 패키지를 설치합니다.

## <a name="regenerate-the-model"></a>모델 다시 생성

이제 리버스 엔지니어링 기능을 사용 하 여 기존 데이터베이스를 기반으로 모델을 만들 수 있습니다.

패키지 관리자 콘솔에서 다음 명령을 실행 합니다 (도구 – > NuGet 패키지 관리자-> 패키지 관리자 콘솔). 테이블의 하위 집합을 스 캐 폴드 하는 명령 옵션은 [패키지 관리자 콘솔 (Visual Studio)](../../core/miscellaneous/cli/powershell.md) 을 참조 하세요.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

예를 들어 SQL Server LocalDB 인스턴스의 블로그 데이터베이스에서 모델을 스 캐 폴드 하는 명령은 다음과 같습니다.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>EF6 model 제거

이제 응용 프로그램에서 EF6 모델을 제거 합니다.

EF Core 및 EF6는 동일한 응용 프로그램에서 함께 사용할 수 있으므로 EF6 NuGet 패키지 (EntityFramework)를 설치 하는 것은 괜찮습니다. 그러나 응용 프로그램의 영역에서 EF6를 사용 하지 않으려는 경우에는 패키지를 제거 하면 주의가 필요한 코드 조각에 컴파일 오류를 제공할 수 있습니다.

## <a name="update-your-code"></a>코드 업데이트

이 시점에서 컴파일 오류를 해결 하 고 코드를 검토 하 여 EF6와 EF Core 간의 동작이 어떻게 영향을 주는지 확인 하는 것이 중요 합니다.

## <a name="test-the-port"></a>포트 테스트

응용 프로그램이 컴파일되는 경우에만가 EF Core에 성공적으로 이식 되었다는 것을 의미 하지는 않습니다. 응용 프로그램의 모든 영역을 테스트 하 여 어떤 동작 변경도 응용 프로그램에 부정적인 영향을 주지 않는지 확인 해야 합니다.
