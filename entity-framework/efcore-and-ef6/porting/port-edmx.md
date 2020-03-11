---
title: EF6에서 EF Core로 이식 - EDMX 기반 모델 이식 - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413530"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>EF6 EDMX 기반 모델을 EF Core로 이식

EF Core는 모델에 대한 EDMX 파일 형식을 지원하지 않습니다. 이러한 모델을 이식하기 위한 최고의 옵션은 애플리케이션의 데이터베이스에서 새 코드 기반 모델을 생성하는 것입니다.

## <a name="install-ef-core-nuget-packages"></a>EF Core NuGet 패키지 설치

`Microsoft.EntityFrameworkCore.Tools` NuGet 패키지를 설치합니다.

## <a name="regenerate-the-model"></a>모델 다시 생성

이제 리버스 엔지니어링 기능을 사용하여 기존 데이터베이스를 기반의 모델을 만들 수 있습니다.

패키지 관리자 콘솔(도구 –> NuGet 패키지 관리자 -> 패키지 관리자 콘솔)에서 다음 명령을 실행합니다. 테이블의 하위 집합 등을 스캐폴드하기 위한 명령 옵션은 [패키지 관리자 콘솔(Visual Studio)](../../core/miscellaneous/cli/powershell.md)을 참조하세요.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

예를 들어 SQL Server LocalDB 인스턴스의 블로깅 데이터베이스에서 모델을 스캐폴드하기 위한 명령은 다음과 같습니다.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>EF6 모델 제거

이제 애플리케이션에서 EF6 모델을 제거합니다.

EF Core 및 EF6는 동일한 애플리케이션에서 함께 사용할 수 있으므로 EF6 NuGet 패키지(EntityFramework)가 설치하는 것은 괜찮습니다. 그러나 애플리케이션의 영역에서 EF6를 사용하지 않으려는 경우에는 패키지를 제거하면 주의가 필요한 코드 조각에 컴파일 오류를 제공할 수 있습니다.

## <a name="update-your-code"></a>코드 업데이트

이 시점에서 컴파일 오류를 해결하고 코드를 검토하여 EF6과 EF Core 간의 동작 변경이 어떤 영향을 주는지 확인하는 것이 중요합니다.

## <a name="test-the-port"></a>포트 테스트

애플리케이션이 컴파일되는 것만으로 EF Core에 성공적으로 이식되었음을 의미하지는 않습니다. 애플리케이션의 모든 영역을 테스트하여 어떠한 동작 변경도 애플리케이션에 부정적인 영향을 주지 않았음을 확인해야 합니다.
