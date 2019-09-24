---
title: 시작 - EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 41ebdcbb3f51c914ee7befb3c1a9c0042e9b43c8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196891"
---
# <a name="getting-started-with-ef-core"></a>EF Core 시작

이 자습서에서는 Entity Framework Core를 사용하여 SQLite 데이터베이스에 대한 데이터 액세스를 수행하는 .NET Core 콘솔 앱을 만듭니다.

Windows에서 Visual Studio를 사용하거나 Windows, macOS 또는 Linux에서 .NET Core CLI를 사용하여 자습서를 진행할 수 있습니다.

[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).

## <a name="prerequisites"></a>전제 조건

다음 소프트웨어를 설치합니다.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* [.NET Core 3.0 SDK](https://www.microsoft.com/net/download/core)

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 이 워크로드의 경우 [Visual Studio 2019 버전 16.3 이상](https://www.visualstudio.com/downloads/):
  * **.NET Core 플랫폼 간 개발**(**기타 도구 집합** 아래)

---

## <a name="create-a-new-project"></a>새 프로젝트 만들기

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

``` Console
dotnet new console -o EFGetStarted
cd EFGetStarted
```

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio를 엽니다.
* **새 프로젝트 만들기**를 클릭합니다.
* **C#** 태그가 있는 **콘솔 앱(.NET Core)** 을 선택하고 **다음**을 클릭합니다
* 이름에 **EFGetStarted**를 입력하고 **만들기**를 클릭합니다.

---

## <a name="install-entity-framework-core"></a>Entity Framework Core 설치

EF Core를 설치하려면 대상으로 지정할 EF Core 데이터베이스 공급자에 대한 패키지를 설치합니다. 이 자습서에서는 .NET Core가 지원하는 모든 플랫폼에서 실행되는 SQLite를 사용합니다. 사용 가능한 공급자 목록은 [데이터베이스 공급자](../providers/index.md)를 참조하세요.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**
* 다음 명령을 실행합니다.

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

팁: 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택하여 패키지를 설치할 수도 있습니다.

---

## <a name="create-the-model"></a>모델 만들기

모델을 구성하는 컨텍스트 클래스 및 엔터티 클래스를 정의합니다.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* 프로젝트 디렉터리에서 다음 코드를 사용하여 **Model.cs**를 만듭니다.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가 > 클래스**를 선택합니다.
* 이름으로 **Model.cs**를 입력하고 **추가**를 클릭합니다.
* 파일의 내용을 다음 코드로 바꿉니다.

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

EF Core는 모델을 기존 데이터베이스에서 [리버스 엔지니어링](../managing-schemas/scaffolding.md)할 수도 있습니다.

팁: 실제 앱에서는 각 클래스를 별도의 파일에 저장하고 구성 파일 또는 환경 변수에 [연결 문자열](../miscellaneous/connection-strings.md)을 저장합니다. 자습서를 간단히 유지하기 위해 모든 항목이 하나의 파일에 포함되어 있습니다.

## <a name="create-the-database"></a>데이터베이스 만들기

다음 단계에서는 [마이그레이션](xref:core/managing-schemas/migrations/index)을 사용하여 데이터베이스를 만듭니다.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* 다음 명령을 실행합니다.

  ``` Console
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  그러면 [dotnet ef](../miscellaneous/cli/dotnet.md)와 프로젝트에서 명령을 실행하는 데 필요한 디자인 패키지가 설치됩니다. `migrations` 명령은 마이그레이션을 스캐폴딩하여 모델에 대한 초기 테이블 집합을 만듭니다. `database update` 명령은 데이터베이스를 만들고 새 마이그레이션을 적용합니다.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **패키지 관리자 콘솔**에서 다음 명령을 실행합니다.

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  그러면 [EF Core용 PMC 도구](../miscellaneous/cli/powershell.md)가 설치됩니다. `Add-Migration` 명령은 마이그레이션을 스캐폴딩하여 모델에 대한 초기 테이블 집합을 만듭니다. `Update-Database` 명령은 데이터베이스를 만들고 새 마이그레이션을 적용합니다.

---

## <a name="create-read-update--delete"></a>만들기, 읽기, 업데이트 및 삭제

* *Program.cs*를 열고 내용을 다음 코드로 바꿉니다.

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a>앱 실행

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

``` Console
dotnet run
```

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio는 .NET Core 콘솔 앱을 실행할 때 일관되지 않은 작업 디렉터리를 사용합니다. ([dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619) 참조) 이로 인해 다음과 같은 예외가 발생합니다. *해당 테이블이 없습니다. Blogs*. 작업 디렉터리를 업데이트하려면

* 프로젝트를 마우스 오른쪽 단추로 클릭하고 **프로젝트 파일 편집**을 선택합니다.
* *Targetframework* 속성 바로 아래에 다음을 추가합니다.

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* 파일 저장

이제 앱을 실행할 수 있습니다.

* **디버그 > 디버깅하지 않고 시작**

---

# <a name="next-steps"></a>다음 단계

* [ASP.NET Core 자습서](/aspnet/core/data/ef-rp/intro)에 따라 웹앱에서 EF Core를 사용합니다.
* [LINQ 쿼리 식](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)에 대해 자세히 알아봅니다.
* [모델을 구성](xref:core/modeling/index)하여 [필수](xref:core/modeling/required-optional) 및 [최대 길이](xref:core/modeling/max-length)와 같은 항목을 지정합니다.
* 모델을 변경한 후 [마이그레이션](xref:core/managing-schemas/migrations/index)을 사용하여 데이터베이스 스키마를 업데이트합니다.
