---
title: "디자인 타임 DbContext 만들기-EF 코어"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a>디자인 타임 DbContext 만들기
==============================
명령에는 디자인에 만들려는 DbContext 인스턴스 필요 EF 도구 중 일부 (예를 들어 실행 하는 경우 마이그레이션 명령) 시간입니다. 여러 가지 방법으로 도구를 만들려고 시도 합니다.

<a name="from-application-services"></a>응용 프로그램 서비스
-------------------------
시작 프로젝트는 ASP.NET Core 응용 프로그램, 도구는 응용 프로그램의 서비스 공급자에서 DbContext 개체를 가져올 하려고 합니다. 호출 하 여 가져올은 `Program.BuildWebHost()` 에 액세스 하 고는 `IWebHost.Services` 속성입니다. 사용 하 여 모든 DbContext 등록 `IServiceCollection.AddDbContext<TContext>()` 찾을 수 있으며 이런 방식이으로 만든 합니다. 이 패턴 [ASP.NET 코어 2.0에 도입 된][1]

<a name="using-the-default-constructor"></a>기본 생성자를 사용 하 여
-----------------------------
DbContext를 응용 프로그램 서비스 공급자 로부터 가져올 수 없으며 도구 프로젝트 내부에 DbContext 형식 찾아보십시오. 기본 생성자를 사용 하 여 만들 하려고 합니다.

<a name="from-a-design-time-factory"></a>디자인 타임 공장에서
--------------------------
알 수 있습니다는 도구를 구현 하 여 프로그램 DbContext를 만드는 방법을 `IDesignTimeDbContextFactory`합니다. 이 인터페이스를 구현 하는 클래스는 프로젝트 내부에서 발견 되 면 도구 DbContext를 만드는 다른 방법으로 무시 합니다.
팩터리 디자인 타임에 항상 사용합니다. 팩터리는 런타임 시 보다 디자인 타임에 대 한 DbContext를 다르게 구성 해야 하는 경우에 특히 유용 합니다.

``` csharp
using Microsoft.EntityFrameworkCore;
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
> `args` 매개 변수는 현재 사용 되지 않습니다. [문제] [ 2] 도구에서 디자인 타임 인수를 지정 하는 기능을 추적 합니다.

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
