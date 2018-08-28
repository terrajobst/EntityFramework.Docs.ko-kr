---
title: EF6에서 EF Core로 이식-코드 기반 모델 이식
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997051"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>EF core는 EF6 코드 기반 모델 이식

모든 주의 읽은 경우 포트 준비가 다음 다음은 시작 하는 데 도움이 되는 몇 가지 지침입니다.

## <a name="install-ef-core-nuget-packages"></a>EF Core NuGet 패키지를 설치 합니다.

EF Core를 사용 하려면 사용 하려는 데이터베이스 공급자에 대 한 NuGet 패키지를 설치 합니다. 예를 들어, SQL Server를 대상으로 하는 경우 설치 하는 것 `Microsoft.EntityFrameworkCore.SqlServer`입니다. 참조 [데이터베이스 공급자](../../core/providers/index.md) 세부 정보에 대 한 합니다.

마이그레이션을 사용 하려는 경우도 설치 해야 합니다 `Microsoft.EntityFrameworkCore.Tools` 패키지 있습니다.

EF Core 및 EF6 사용된-side-by-side 동일한 응용 프로그램 수 설치 (EntityFramework) EF6 NuGet에서 패키지를 그대로 두는 것이 괜찮습니다. 그러나 EF6 응용 프로그램의 모든 영역에서 사용 하려는 없습니다, 하는 경우 다음 패키지를 제거는 데 도움이 됩니다 주의가 필요한 코드 부분에 컴파일 오류가 발생 합니다.

## <a name="swap-namespaces"></a>네임 스페이스를 교환 합니다.

EF6에서 사용 하는 대부분의 Api에는 `System.Data.Entity` 네임 스페이스 (및 하위 네임 스페이스 관련)입니다. 첫 번째 코드 변경을 교환 하는 것은 `Microsoft.EntityFrameworkCore` 네임 스페이스입니다. 일반적으로 파생된 컨텍스트 코드 파일을 시작 하는 작업 해야 여기에서 발생 하는 컴파일 오류를 해결 합니다.

## <a name="context-configuration-connection-etc"></a>상황에 맞는 구성 (연결 등.)

에 설명 된 대로 [보장 EF Core는 작업 응용 프로그램에 대 한](ensure-requirements.md), EF Core가 덜 magic 주위에 연결할 데이터베이스를 검색 합니다. 재정의 해야 합니다는 `OnConfiguring` 데이터베이스 공급자 특정 API 사용 하 여 데이터베이스로 연결을 설정 하 고 파생된 컨텍스트 메서드.

응용 프로그램에서 연결 문자열을 저장 하는 대부분의 EF6 응용 프로그램 `App/Web.config` 파일입니다. EF Core에서 읽을 있습니다 사용 하 여이 연결 문자열을 `ConfigurationManager` API. 에 대 한 참조를 추가 해야 합니다 `System.Configuration` 프레임 워크 어셈블리를이 API를 사용 하는 일을 할 수 있습니다.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a>코드 업데이트

이 시점에서 컴파일 오류를 해결 하는 경우 변경 된 동작은 영향을 확인 하려면 코드를 검토 한 문제는 것입니다.

## <a name="existing-migrations"></a>기존 마이그레이션

기존 EF6 마이그레이션 EF Core로 이식 가능 하면 실제로 없습니다.

가능한 경우 EF6의 모든 이전 마이그레이션 데이터베이스에 적용 된 스키마는 마이그레이션 시작 가리킨 EF Core를 사용 하 여 가정 하는 것이 좋습니다. 이 작업을 수행 하려면 사용 된 `Add-Migration` 모델 EF Core로 이식 되 면 마이그레이션을 추가 하는 명령입니다. 다음의 모든 코드를 제거 합니다 `Up` 및 `Down` 스 캐 폴드 된 마이그레이션 메서드. 이후 마이그레이션 해당 초기 마이그레이션을 스 캐 폴드 된 경우 모델을 비교 합니다.

## <a name="test-the-port"></a>포트를 테스트 합니다.

응용 프로그램을 컴파일합니다. 단지 EF Core로 이식 성공적으로 의미 하지 않습니다. 동작 변경 내용이 부정적인 영향을 준 응용 프로그램을 확인 하도록 응용 프로그램의 모든 영역을 테스트 해야 합니다.
