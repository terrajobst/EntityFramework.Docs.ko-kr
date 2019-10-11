---
title: EF6에서 EF Core로 포팅-코드 기반 모델 포팅-EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181224"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>EF Core EF6 코드 기반 모델 포팅

모든 주의 사항을 읽고 포트 준비가 완료 되 면 시작 하는 데 도움이 되는 몇 가지 지침을 제공 합니다.

## <a name="install-ef-core-nuget-packages"></a>NuGet 패키지 EF Core 설치

EF Core를 사용 하려면 사용 하려는 데이터베이스 공급자에 대 한 NuGet 패키지를 설치 합니다. 예를 들어 SQL Server를 대상으로 지정 하는 경우 `Microsoft.EntityFrameworkCore.SqlServer`을 설치 합니다. 자세한 내용은 [데이터베이스 공급자](../../core/providers/index.md) 를 참조 하세요.

마이그레이션을 사용 하려는 경우 `Microsoft.EntityFrameworkCore.Tools` 패키지도 설치 해야 합니다.

EF Core 및 EF6는 동일한 응용 프로그램에서 함께 사용할 수 있으므로 EF6 NuGet 패키지 (EntityFramework)를 설치 하는 것은 괜찮습니다. 그러나 응용 프로그램의 영역에서 EF6를 사용 하지 않으려는 경우에는 패키지를 제거 하면 주의가 필요한 코드 조각에 컴파일 오류를 제공할 수 있습니다.

## <a name="swap-namespaces"></a>네임 스페이스 바꾸기

EF6에서 사용 하는 대부분의 Api는 `System.Data.Entity` 네임 스페이스와 관련 하위 네임 스페이스에 있습니다. 첫 번째 코드 변경 내용은 `Microsoft.EntityFrameworkCore` 네임 스페이스로 교체 하는 것입니다. 일반적으로 파생 컨텍스트 코드 파일로 시작 하 여 발생 하는 컴파일 오류를 해결 하는 작업을 수행 합니다.

## <a name="context-configuration-connection-etc"></a>컨텍스트 구성 (연결 등)

[응용 프로그램에 EF Core 작동 하는지 확인](ensure-requirements.md)에서 설명한 것 처럼 EF Core에 연결할 데이터베이스를 검색 하는 것이 더 나을 수 있습니다. 파생 된 컨텍스트에서 `OnConfiguring` 메서드를 재정의 하 고 데이터베이스 공급자 특정 API를 사용 하 여 데이터베이스에 대 한 연결을 설정 해야 합니다.

대부분의 EF6 응용 프로그램은 응용 프로그램 `App/Web.config` 파일에 연결 문자열을 저장 합니다. EF Core에서 `ConfigurationManager` API를 사용 하 여이 연결 문자열을 읽습니다. 이 API를 사용하려면 `System.Configuration` 프레임워크 어셈블리에 대한 참조를 추가해야 할 수 있습니다.

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

이 시점에서 컴파일 오류를 해결 하 고 코드를 검토 하 여 동작 변경으로 인해 영향을 주는지 확인 하는 것이 중요 합니다.

## <a name="existing-migrations"></a>기존 마이그레이션

기존 EF6 마이그레이션을 EF Core로 이식 하는 것은 사실상 적절 한 방법이 아닙니다.

가능 하면 EF6의 모든 이전 마이그레이션이 데이터베이스에 적용 된 후 EF Core를 사용 하 여 해당 지점에서 스키마 마이그레이션을 시작 하는 것이 가장 좋습니다. 이렇게 하려면 모델을 EF Core로 이식 하 고 나면 `Add-Migration` 명령을 사용 하 여 마이그레이션을 추가 합니다. 그런 다음 스 캐 폴드 마이그레이션의 `Up` 및 `Down` 메서드에서 모든 코드를 제거 합니다. 이후 마이그레이션은 초기 마이그레이션이 스 캐 폴드 때의 모델과 비교 됩니다.

## <a name="test-the-port"></a>포트 테스트

응용 프로그램이 컴파일되는 경우에만가 EF Core에 성공적으로 이식 되었다는 것을 의미 하지는 않습니다. 응용 프로그램의 모든 영역을 테스트 하 여 어떤 동작 변경도 응용 프로그램에 부정적인 영향을 주지 않는지 확인 해야 합니다.
