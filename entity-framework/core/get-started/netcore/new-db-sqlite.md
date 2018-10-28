---
title: .NET Core에서 시작 - 새 데이터베이스 - EF Core
author: rick-anderson
ms.author: riande
description: Entity Framework Core를 사용하여 .NET Core 시작
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 6cebe14e179cb6998592f5d3823c114b3bda0138
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022313"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>.NET Core 콘솔 앱에서 새 데이터베이스로 EF Core 시작

이 자습서에서는 Entity Framework Core를 사용하여 SQLite 데이터베이스에 대한 데이터 액세스를 수행하는 .NET Core 콘솔 앱을 만듭니다. 마이그레이션을 사용하여 모델에서 데이터베이스를 만듭니다. ASP.NET Core MVC를 사용하는 Visual Studio 버전에 대해서는 [ASP.NET Core - 새 데이터베이스](xref:core/get-started/aspnetcore/new-db)를 참조하세요.

[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).

## <a name="prerequisites"></a>전제 조건

* [.NET Core 2.1 SDK](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>새 프로젝트 만들기

* 새 콘솔 프로젝트를 만듭니다.

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a>현재 디렉터리 변경

이후 단계에서는 응용 프로그램에 대해 `dotnet` 명령을 실행해야 합니다.

* 현재 디렉터리를 다음과 같은 응용 프로그램 디렉터리로 변경합니다.

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a>Entity Framework Core 설치

EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다. 이 연습에서는 SQLite를 사용합니다. 사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.

* Microsoft.EntityFrameworkCore.Sqlite 및 Microsoft.EntityFrameworkCore.Design 설치

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* `dotnet restore`를 실행하여 새 패키지를 설치합니다.

## <a name="create-the-model"></a>모델 만들기

모델을 구성하는 컨텍스트 및 엔터티 클래스를 정의합니다.

* 다음 콘텐츠를 사용하여 새 *Model.cs* 파일을 만듭니다.

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

팁: 실제 응용 프로그램에서는 각 클래스를 별도의 파일에 저장하고 구성 파일 또는 환경 변수에 연결 문자열을 저장합니다. 자습서를 간단히 유지하기 위해 모든 항목이 하나의 파일에 포함되어 있습니다.

## <a name="create-the-database"></a>데이터베이스 만들기

모델을 만든 후 [마이그레이션](xref:core/managing-schemas/migrations/index)을 사용하여 데이터베이스를 만듭니다.

* `dotnet ef migrations add InitialCreate`를 실행하여 마이그레이션을 스캐폴딩하고 모델에 대한 초기 테이블 집합을 만듭니다.
* `dotnet ef database update`를 실행하여 새 마이그레이션을 데이터베이스에 적용합니다. 이 명령은 마이그레이션을 적용하기 전에 데이터베이스를 만듭니다.

*blogging.db** SQLite DB는 프로젝트 디렉터리에 있습니다.

## <a name="use-the-model"></a>모델 사용

* *Program.cs*를 열고 내용을 다음 코드로 바꿉니다.

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* 콘솔에서 앱을 테스트합니다. Visual Studio에서 앱을 실행하려면 [Visual Studio 참고](#vs)를 참조하세요.

  `dotnet run`

  하나의 블로그가 데이터베이스에 저장되고 모든 블로그의 세부 정보가 콘솔에 표시됩니다.

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>모델 변경:

- 모델을 변경하는 경우 `dotnet ef migrations add` 명령을 사용하여 [마이그레이션](xref:core/managing-schemas/migrations/index)을 스캐폴딩할 수 있습니다. 스캐폴딩된 코드를 확인하고 필요한 내용을 변경한 후 `dotnet ef database update` 명령을 사용하여 스키마 변경 내용을 데이터베이스에 적용할 수 있습니다.
- EF Core는 데이터베이스의 `__EFMigrationsHistory` 테이블을 사용하여 이미 데이터베이스에 적용된 마이그레이션을 추적합니다.
- SQLite 데이터베이스 엔진은 대부분의 다른 관계형 데이터베이스에서 지원하는 특정 스키마 변경을 지원하지 않습니다. 예를 들어 `DropColumn` 작업은 지원되지 않습니다. EF Core 마이그레이션은 이러한 작업에 대한 코드를 생성합니다. 그러나 데이터베이스에 적용하거나 스크립트를 생성하려고 하면 EF Core에서 예외를 throw합니다. [SQLite 제한 사항](../../providers/sqlite/limitations.md)을 참조하세요. 새 개발의 경우 모델이 변경될 때 마이그레이션을 사용하는 대신 데이터베이스를 삭제하고 새 개발을 만드는 것이 좋습니다.

<a name="vs"></a>
### <a name="run-from-visual-studio"></a>Visual Studio에서 실행

이 샘플을 Visual Studio에서 실행하려면 수동으로 작업 디렉터리를 프로젝트의 루트로 설정해야 합니다. 작업 디렉터리를 설정하지 않으면 다음 `Microsoft.Data.Sqlite.SqliteException`이 throw됩니다. `SQLite Error 1: 'no such table: Blogs'`

작업 디렉터리를 설정하려면

* **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.
* 왼쪽 창에서 **디버그** 탭을 선택합니다.
* **작업 디렉터리**를 프로젝트 디렉터리로 설정합니다.
* 변경 내용을 저장합니다.

## <a name="additional-resources"></a>추가 리소스

* [자습서: SQLite를 사용하여 ASP.NET Core에서 새 데이터베이스로 EF Core 시작](xref:core/get-started/aspnetcore/new-db)
* [자습서: ASP.NET Core에서 Razor Pages 시작](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [자습서: ASP.NET Core에서 Entity Framework Core를 사용한 Razor 페이지](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
