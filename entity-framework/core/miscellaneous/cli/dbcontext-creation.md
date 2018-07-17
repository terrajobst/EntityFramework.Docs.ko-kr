---
title: 디자인 타임 DbContext 만들기-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 7c16017d3b97d115841050fe6ac0fdbeb5e71d94
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067533"
---
<a name="design-time-dbcontext-creation"></a>디자인 타임 DbContext 만들기
==============================
EF Core 도구 명령 중 일부 (예를 들어,를 [마이그레이션] [ 1] 명령) 파생 필요 `DbContext` 응용 프로그램에 대 한 세부 정보를 수집 하기 위해 디자인 타임에 만들려는 인스턴스 엔터티 형식 및 데이터베이스 스키마에 매핑하는 방법입니다. 대부분의 경우에서 것이 바람직한는 `DbContext` 있으므로 만든 어떻게 것을 비슷한 방식으로 구성 된 [런타임에 구성][2]합니다.

여러 가지 도구를 만들려고 시도 합니다 `DbContext`:

<a name="from-application-services"></a>응용 프로그램 서비스에서
-------------------------
시작 프로젝트는 ASP.NET Core 앱 인 경우 도구는 응용 프로그램의 서비스 공급자에서 DbContext 개체를 가져올 하려고 합니다.

도구를 호출 하 여 서비스 공급자를 가져오려면 먼저 `Program.BuildWebHost()` 에 액세스 하 고는 `IWebHost.Services` 속성입니다.

> [!NOTE]
> 새 ASP.NET Core 2.0 응용 프로그램을 만들면 기본적으로이 연결 고리 포함 됩니다. EF Core 및 ASP.NET Core의 이전 버전에서는 도구를 호출 하려고 `Startup.ConfigureServices` 더 이상 응용 프로그램의 서비스 공급자에 있지만이 패턴을 얻기 위해 제대로 작동 ASP.NET Core 2.0 응용 프로그램에서 직접. 2.0에는 ASP.NET Core 1.x 응용 프로그램을 업그레이드 하는 경우 [수정 하 `Program` 새 패턴을 따르는 클래스][3]합니다.

`DbContext` 자체 및 해당 생성자에서 종속성 서비스 응용 프로그램의 서비스 공급자에 등록 해야 합니다. 함으로써 쉽게 수행할 수 있습니다 [에 있는 생성자는 `DbContext` 의 인스턴스를 사용 하 `DbContextOptions<TContext>` 인수로] [ 4] 사용 하는 [ `AddDbContext<TContext>` 메서드] [5].

<a name="using-a-constructor-with-no-parameters"></a>매개 변수가 없는 생성자를 사용 하 여
--------------------------------------
DbContext를 응용 프로그램 서비스 공급자에서 가져올 수 없습니다. 도구를 찾아보십시오 파생 `DbContext` 프로젝트 형식. 다음 매개 변수가 없는 생성자를 사용 하 여 인스턴스를 만들려고 시도 합니다. 이 경우 기본 생성자를 수 있습니다 합니다 `DbContext` 을 사용 하도록 구성 합니다 [ `OnConfiguring` ] [ 6] 메서드.

<a name="from-a-design-time-factory"></a>디자인 타임 팩터리에서
--------------------------
알 수 있습니다 도구를 구현 하 여 DbContext를 만드는 방법 합니다 `IDesignTimeDbContextFactory<TContext>` 인터페이스: 있으면이 인터페이스를 구현 하는 클래스에서 파생 된 동일한 프로젝트 `DbContext` 또는 응용 프로그램의 시작 프로젝트에서 도구는 다음과 같이 무시 됩니다. 대신 DbContext 및 디자인 타임 팩터리를 사용 하 여 만드는 다른 방법입니다.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

> [!NOTE]
> `args` 매개 변수가 현재 사용 되지 않습니다. 있기 [문제일] [ 7] 도구에서 디자인 타임에 인수를 지정 하는 기능을 추적 합니다.

디자인 타임 팩터리 경우 런타임 시 보다 디자인 타임에 대 한 DbContext를 다르게 구성 해야 하는 경우에 특히 유용할 수 있습니다는 `DbContext` 생성자는 DI를 전혀 사용 하지 않는 경우 추가 매개 변수 DI를에 등록 되지 않은 또는 일부에 대 한 경우 유지할 필요가 없으며 하려는 이유는 `BuildWebHost` ASP.NET Core 응용 프로그램의 메서드 `Main` 클래스입니다.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
