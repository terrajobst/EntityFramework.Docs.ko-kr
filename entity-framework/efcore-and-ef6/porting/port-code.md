---
title: EF6에서 EF Core로 이식 - 코드 기반 모델 이식 - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413860"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>EF6 코드 기반 모델을 EF Core로 이식

모든 주의 사항을 읽고 포트 준비가 완료되었으면 시작하는 데 도움이 되는 몇 가지 지침을 확인하세요.

## <a name="install-ef-core-nuget-packages"></a>EF Core NuGet 패키지 설치

EF Core를 사용하려면 사용할 데이터베이스 공급자에 대한 NuGet 패키지를 설치합니다. 예를 들어 SQL Server를 대상으로 지정하는 경우 `Microsoft.EntityFrameworkCore.SqlServer`를 설치합니다. 자세한 내용은 [데이터베이스 공급자](../../core/providers/index.md)를 참조하세요.

마이그레이션을 사용하려는 경우에는 `Microsoft.EntityFrameworkCore.Tools` 패키지도 설치해야 합니다.

EF Core 및 EF6는 동일한 애플리케이션에서 함께 사용할 수 있으므로 EF6 NuGet 패키지(EntityFramework)가 설치하는 것은 괜찮습니다. 그러나 애플리케이션의 영역에서 EF6를 사용하지 않으려는 경우에는 패키지를 제거하면 주의가 필요한 코드 조각에 컴파일 오류를 제공할 수 있습니다.

## <a name="swap-namespaces"></a>네임스페이스 전환

EF6에서 사용하는 대부분의 API는 `System.Data.Entity` 네임스페이스(및 관련 하위 네임스페이스)에 있습니다. 첫 번째 코드 변경은 `Microsoft.EntityFrameworkCore` 네임스페이스로 전환하는 것입니다. 일반적으로 파생 컨텍스트 코드 파일로 시작하여 발생하는 컴파일 오류를 해결하면서 작업을 수행합니다.

## <a name="context-configuration-connection-etc"></a>컨텍스트 구성(연결 등)

[EF Core가 애플리케이션에 대해 작동하는지 확인](ensure-requirements.md)에 설명한 대로 EF Core에는 연결할 데이터베이스를 감지하는 기능이 부족합니다. 파생 컨텍스트에서 `OnConfiguring` 메서드를 재정의하고 데이터베이스 공급자별 API를 사용하여 데이터베이스에 대한 연결을 설정해야 합니다.

대부분의 EF6 애플리케이션은 애플리케이션 `App/Web.config` 파일에 연결 문자열을 저장합니다. EF Core에서는 `ConfigurationManager` API를 사용하여 이 연결 문자열을 읽습니다. 이 API를 사용하려면 `System.Configuration` 프레임워크 어셈블리에 대한 참조를 추가해야 할 수 있습니다.

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

이 시점에서 컴파일 오류를 해결하고 코드를 검토하여 동작 변경이 어떤 영향을 주는지 확인하는 것이 중요합니다.

## <a name="existing-migrations"></a>기존 마이그레이션

기존의 EF6 마이그레이션을 EF Core로 이식하는 것은 사실상 적절한 방법이 아닙니다.

가능하면 EF6의 모든 이전 마이그레이션이 데이터베이스에 적용된 후 EF Core를 사용하여 해당 지점에서 스키마 마이그레이션을 시작하는 것이 가장 좋습니다. 이렇게 하려면 모델을 EF Core로 이식할 때 `Add-Migration` 명령을 사용하여 마이그레이션을 추가합니다. 그런 다음 스캐폴드 마이그레이션의 `Up` 및 `Down` 메서드에서 모든 코드를 제거합니다. 후속 마이그레이션은 초기 마이그레이션이 스캐폴드되었을 때의 모델과 비교됩니다.

## <a name="test-the-port"></a>포트 테스트

애플리케이션이 컴파일되는 것만으로 EF Core에 성공적으로 이식되었음을 의미하지는 않습니다. 애플리케이션의 모든 영역을 테스트하여 어떠한 동작 변경도 애플리케이션에 부정적인 영향을 주지 않았음을 확인해야 합니다.
