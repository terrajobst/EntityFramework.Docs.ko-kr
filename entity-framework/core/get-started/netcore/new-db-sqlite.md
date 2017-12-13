---
title: ".NET Core에서 시작 - 새 데이터베이스 - EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: "Entity Framework Core를 사용하여 .NET Core 시작"
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 22fc0446dee71dd0d2402b47d76cc8b7307fbe5f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>.NET Core 콘솔 앱에서 새 데이터베이스로 EF Core 시작

이 연습에서는 Entity Framework Core를 사용하여 SQLite 데이터베이스에 대해 기본 데이터 액세스를 수행하는 .NET Core 콘솔 앱을 만듭니다. 마이그레이션을 사용하여 모델에서 데이터베이스를 만듭니다. ASP.NET Core MVC를 사용하는 Visual Studio 버전에 대해서는 [ASP.NET Core - 새 데이터베이스](xref:core/get-started/aspnetcore/new-db)를 참조하세요.

> [!NOTE]  
> [.NET Core SDK](https://www.microsoft.com/net/download/core)는 더 이상 `project.json` 또는 Visual Studio 2015를 지원하지 않습니다. [project.json에서 csproj로 마이그레이션](https://docs.microsoft.com/dotnet/articles/core/migration/)하는 것이 좋습니다. Visual Studio를 사용하는 경우 [Visual Studio 2017](https://www.visualstudio.com/downloads/)로 마이그레이션하는 것이 좋습니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite)을 볼 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 연습을 완료하려면 다음 필수 구성 요소가 필요합니다.
* .NET Core를 지원하는 운영 체제.
* [.NET Core SDK](https://www.microsoft.com/net/core) 2.0(몇 가지 수정 사항이 있지만 이전 버전으로 응용 프로그램을 만드는 데 지침이 사용될 수 있음).

## <a name="create-a-new-project"></a>새 프로젝트 만들기

* 프로젝트에 대한 새로운 `ConsoleApp.SQLite` 폴더를 만들고 `dotnet` 명령을 사용하여 .NET Core 앱으로 채웁니다.

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a>Entity Framework Core 설치

EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다. 이 연습에서는 SQLite를 사용합니다. 사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.

* Microsoft.EntityFrameworkCore.Sqlite 및 Microsoft.EntityFrameworkCore.Design 설치

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* `ConsoleApp.SQLite.csproj`를 수동으로 편집하여 DotNetCliToolReference를 Microsoft.EntityFrameworkCore.Tools.DotNet에 추가합니다.

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

 참고: 이후 버전의 `dotnet`은 `dotnet add tool`을 통해 DotNetCliToolReferences를 지원합니다.

이제 `ConsoleApp.SQLite.csproj`에 다음이 포함됩니다.

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 참고: 위에서 사용된 버전 번호는 게시 시에 올바른 것입니다.

*  `dotnet restore`를 실행하여 새 패키지를 설치합니다.

## <a name="create-the-model"></a>모델 만들기

모델을 구성하는 컨텍스트 및 엔터티 클래스를 정의합니다.

* 다음 콘텐츠를 사용하여 새 *Model.cs* 파일을 만듭니다.

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

실제 응용 프로그램에서는 각 클래스를 별도의 파일에 저장하고 구성 파일에 연결 문자열을 저장합니다. 자습서를 간단히 유지하기 위해 모든 항목을 하나의 파일에 저장합니다.

## <a name="create-the-database"></a>데이터베이스 만들기

모델을 만든 후 [마이그레이션](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)을 사용하여 데이터베이스를 만들 수 있습니다.

* `dotnet ef migrations add InitialCreate`를 실행하여 마이그레이션을 스캐폴딩하고 모델에 대한 초기 테이블 집합을 만듭니다.
* `dotnet ef database update`를 실행하여 새 마이그레이션을 데이터베이스에 적용합니다. 이 명령은 마이그레이션을 적용하기 전에 데이터베이스를 만듭니다.

> [!NOTE]  
> SQLite에서 상대 경로를 사용하는 경우 경로는 응용 프로그램의 주 어셈블리를 기준으로 합니다. 이 샘플에서는 기본 바이너리가 `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`이므로 SQLite 데이터베이스가 `bin/Debug/netcoreapp2.0/blogging.db`에 있습니다.

## <a name="use-your-model"></a>모델 사용

* *Program.cs*를 열고 내용을 다음 코드로 바꿉니다.

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* 앱을 테스트합니다.

 `dotnet run`

 하나의 블로그가 데이터베이스에 저장되고 모든 블로그의 세부 정보가 콘솔에 표시됩니다.

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>모델 변경:

- 모델을 변경할 경우 `dotnet ef migrations add` 명령을 사용하여 새 [마이그레이션](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)을 스캐폴딩하고 데이터베이스에서 해당 스키마를 변경합니다. 스캐폴딩된 코드를 확인하고 필요한 내용을 변경한 후 `dotnet ef database update` 명령을 사용하여 변경 내용을 데이터베이스에 적용할 수 있습니다.
- EF는 데이터베이스의 `__EFMigrationsHistory` 테이블을 사용하여 이미 데이터베이스에 적용된 마이그레이션을 추적합니다.
- SQLite의 제한 사항으로 인해 SQLite는 일부 마이그레이션(스키마 변경)을 지원하지 않습니다. [SQLite 제한 사항](../../providers/sqlite/limitations.md)을 참조하세요. 새 개발의 경우 모델이 변경될 때 마이그레이션을 사용하는 대신 데이터베이스를 삭제하고 새 개발을 만드는 것이 좋습니다.

## <a name="additional-resources"></a>추가 리소스

* [.NET Core - SQLite를 사용하여 새 데이터베이스 만들기](xref:core/get-started/netcore/new-db-sqlite) - 플랫폼 간 콘솔 EF 자습서.
* [Mac 또는 Linux의 ASP.NET Core MVC 소개](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Visual Studio의 ASP.NET Core MVC 소개](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Visual Studio를 사용하여 ASP.NET Core 및 Entity Framework Core 시작](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
