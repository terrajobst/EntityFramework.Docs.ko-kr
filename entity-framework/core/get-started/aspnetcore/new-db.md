---
title: ASP.NET Core에서 시작 - 새 데이터베이스 - EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 25e5a683acf4bbed0b978cc6a80f1b50a0b64ca1
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319181"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>ASP.NET Core에서 새 데이터베이스로 EF Core 시작

이 자습서에서는 Entity Framework Core를 사용하여 기본 데이터 액세스를 수행하는 ASP.NET Core MVC 애플리케이션을 빌드합니다. 이 자습서에서는 마이그레이션을 사용하여 데이터 모델에서 데이터베이스를 만듭니다.

Windows에서 Visual Studio 2017을 사용하거나 Windows, macOS 또는 Linux에서 .NET Core CLI를 사용하여 자습서를 진행할 수 있습니다.

GitHub에서 이 문서의 샘플을 봅니다.
* [SQL Server가 있는 Visual Studio 2017](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* [SQLite가 있는 .NET Core CLI](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite)

## <a name="prerequisites"></a>전제 조건

다음 소프트웨어를 설치합니다.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 다음 워크로드가 포함된 [Visual Studio 2017 버전 15.7 이상](https://www.visualstudio.com/downloads/)
  * **ASP.NET 및 웹 개발**(**웹 및 클라우드** 아래)
  * **.NET Core 플랫폼 간 개발**(**기타 도구 집합** 아래)
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).

---

## <a name="create-a-new-project"></a>새 프로젝트 만들기

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio 2017을 엽니다.
* **파일 > 새로 만들기 > 프로젝트**
* 왼쪽 메뉴에서 **설치됨 > Visual C# > .NET Core**를 선택합니다.
* **새 ASP.NET Core 웹 애플리케이션**을 선택합니다.
* 이름으로 **EFGetStarted.AspNetCore.NewDb**를 입력하고 **확인**을 클릭합니다.
* **새 ASP.NET Core 웹 애플리케이션** 대화 상자에서:
  * 드롭다운 목록에서 **.NET Core** 및 **ASP.NET Core 2.1**이 선택되어 있는지 확인합니다.
  * **웹 애플리케이션(모델-뷰-컨트롤러)** 프로젝트 템플릿을 선택합니다.
  * **인증**이 **인증 없음**으로 설정되었는지 확인합니다.
  * **확인** 을 클릭합니다.

경고: **인증**에 **없음** 대신 **개별 사용자 계정**을 사용하는 경우 Entity Framework Core 모델은 `Models\IdentityModel.cs`의 프로젝트에 추가됩니다. 이 자습서에서 배운 기술을 사용하면 두 번째 모델을 추가하거나 기존 모델을 확장하여 엔터티 클래스를 포함하도록 선택할 수 있습니다.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* 다음 명령을 실행하여 MVC 프로젝트를 만듭니다.

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* 프로젝트 디렉터리로 변경합니다. 입력하는 다음 명령은 새 프로젝트에 대해 실행해야 합니다.

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a>Entity Framework Core 설치

EF Core를 설치하려면 대상으로 지정할 EF Core 데이터베이스 공급자에 대한 패키지를 설치합니다. 사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

이 자습서에서는 SQL Server를 사용하기 때문에 공급자 패키지를 설치할 필요가 없습니다. SQL Server 공급자 패키지는 [Microsoft.AspnetCore.App 메타패키지](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1)에 포함되어 있습니다.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

이 자습서에서는 .NET Core가 지원하는 모든 플랫폼에서 실행되는 SQLite를 사용합니다.

* 다음 명령을 실행하여 SQLite 공급자를 설치합니다.

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a>모델 만들기

모델을 구성하는 컨텍스트 클래스 및 엔터티 클래스를 정의합니다.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Models** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 클래스**를 선택합니다.
* 이름으로 **Model.cs**를 입력하고 **확인**을 클릭합니다.
* 파일의 내용을 다음 코드로 바꿉니다.

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* **모델** 폴더에서 다음 코드로 **Model.cs**를 만듭니다.

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

프로덕션 앱은 일반적으로 각 클래스를 별도의 파일에 넣습니다. 편의상 이 자습서는 이러한 클래스를 하나의 파일에 저장합니다.

## <a name="register-the-context-with-dependency-injection"></a>종속성 주입으로 컨텍스트 등록

`BloggingContext`를 MVC 컨트롤러에 사용할 수 있도록 하려면 `Startup.cs`의 서비스로 등록합니다.

애플리케이션 시작 중에 서비스(예: `BloggingContext`)가 [종속성 주입](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html)에 등록되므로 생성자 매개 변수 및 속성을 통해 서비스(예: MVC 컨트롤러)를 사용하는 구성 요소에 자동으로 제공될 수 있습니다.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Startup.cs**에서 다음 `using` 문을 추가합니다.

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* 다음 강조 표시된 코드를 `ConfigureServices` 메서드에 추가합니다.

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* **Startup.cs**에서 다음 `using` 문을 추가합니다.

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* 다음 강조 표시된 코드를 `ConfigureServices` 메서드에 추가합니다.

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

일반적으로 프로덕션 앱은 연결 문자열을 구성 파일 또는 환경 변수에 저장합니다. 편의상 이 자습서는 코드에서 정의했습니다. 자세한 내용은 [연결 문자열](../../miscellaneous/connection-strings.md)을 참조하세요.

## <a name="create-the-database"></a>데이터베이스 만들기

다음 단계에서는 [마이그레이션](xref:core/managing-schemas/migrations/index)을 사용하여 데이터베이스를 만듭니다.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**
* 다음 명령을 실행합니다.

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  `The term 'add-migration' is not recognized as the name of a cmdlet`이라는 오류가 표시되면 Visual Studio를 닫았다가 다시 엽니다.

  `Add-Migration` 명령은 마이그레이션을 스캐폴딩하여 모델에 대한 초기 테이블 집합을 만듭니다. `Update-Database` 명령은 데이터베이스를 만들고 새 마이그레이션을 적용합니다.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* 다음 명령을 실행합니다.

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  `migrations` 명령은 마이그레이션을 스캐폴딩하여 모델에 대한 초기 테이블 집합을 만듭니다. `database update` 명령은 데이터베이스를 만들고 새 마이그레이션을 적용합니다.

---

## <a name="create-a-controller"></a>컨트롤러 만들기

`Blog` 엔터티에 대한 컨트롤러 및 보기를 스캐폴드합니다.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **솔루션 탐색기**에서 **컨트롤러** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 컨트롤러**를 선택합니다.
* **보기 포함 MVC 컨트롤러, Entity Framework 사용**을 선택하고 **추가**를 클릭합니다.
* **모델 클래스**를 **Blog**로 설정하고 **데이터 컨텍스트 클래스**를 **BloggingContext**로 설정합니다.
* **추가**를 클릭합니다.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* 다음 명령을 실행합니다.

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  `tool install` 및 `add package` 명령은 컨트롤러 및 뷰를 스캐폴딩할 수 있는 도구를 설치합니다. `restore` 명령은 모든 프로젝트의 패키지가 다운로드되는지 확인하고 `aspnet-codegenerator` 명령은 스캐폴딩을 수행합니다.
---

스캐폴딩 엔진은 다음 파일을 만듭니다.

* 컨트롤러(*Controllers/BlogsController.cs*)
* 만들기, 삭제, 세부 정보, 편집 및 인덱스 페이지에 대한 Razor 뷰(_Views/Blogs/*.cshtml_)

## <a name="run-the-application"></a>애플리케이션 실행

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **디버그** > **디버깅하지 않고 시작**

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet run
```
---

* `/Blogs`로 이동합니다.

* **새로 만들기** 링크를 사용하여 일부 블로그 항목을 만듭니다.

  ![페이지 만들기](_static/create.png)

* **세부 정보**를 테스트하고, 링크를 **편집** 및 **삭제**합니다.

  ![인덱스 페이지](_static/index-new-db.png)

## <a name="additional-resources"></a>추가 리소스

* [자습서: SQLite를 사용하여 .NET Core에서 새 데이터베이스로 EF Core 시작](xref:core/get-started/netcore/new-db-sqlite)
* [자습서: Get started with Razor Pages in ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)(자습서: ASP.NET Core에서 Razor Pages 시작)
* [자습서: Razor Pages with Entity Framework Core in ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)(자습서: ASP.NET Core에서 Entity Framework Core를 사용한 Razor Pages)
