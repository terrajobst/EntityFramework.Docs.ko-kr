---
title: .NET Framework에서 시작 - 기존 데이터베이스 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: d5c548927b736199c7d6fddc9c74139ca5f6614e
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614417"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>.NET Framework에서 기존 데이터베이스로 EF Core 시작

이 자습서에서는 Entity Framework를 사용하여 Microsoft SQL Server 데이터베이스에 대해 기본 데이터 액세스를 수행하는 콘솔 응용 프로그램을 빌드합니다. 기존 데이터베이스를 리버스 엔지니어링하여 Entity Framework 모델을 만듭니다.

[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).

## <a name="prerequisites"></a>전제 조건

* [Visual Studio 2017 버전 15.7 이상](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a>Blogging 데이터베이스 만들기

이 자습서에서는 LocalDb 인스턴스의 **Blogging** 데이터베이스를 기존 데이터베이스로 사용합니다. **이미 다른 자습서의 일부로 Blogging** 데이터베이스를 만든 경우 다음 단계를 건너뜁니다.

* Visual Studio를 엽니다.

* **도구 > 데이터베이스에 연결...**

* **Microsoft SQL Server**를 선택하고 **계속**을 클릭합니다.

* **서버 이름**으로 **(localdb)\mssqllocaldb**를 입력합니다.

* **데이터베이스 이름**으로 **master**를 입력하고 **확인**을 클릭합니다.

* 이제 master 데이터베이스가 **서버 탐색기**의 **데이터 연결**에 표시됩니다.

* **서버 탐색기**에서 데이터베이스를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.

* 아래 나열된 스크립트를 쿼리 편집기로 복사합니다.

* 쿼리 편집기를 마우스 오른쪽 단추로 클릭하고 **실행**을 선택합니다.

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>새 프로젝트 만들기

* Visual Studio 2017을 엽니다.

* **파일 > 새로 만들기 > 프로젝트...**

* 왼쪽 메뉴에서 **설치됨 > Visual C# > Windows Desktop**을 선택합니다.

* **콘솔 앱(.NET Framework)** 프로젝트 템플릿을 선택합니다.

* 프로젝트가 **.NET Framework 4.6.1** 이상을 대상으로 지정하도록 합니다.

* 프로젝트 이름을 *ConsoleApp.ExistingDb*로 지정하고 **확인**을 클릭합니다.

## <a name="install-entity-framework"></a>Entity Framework 설치

EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다. 이 자습서에서는 SQL Server를 사용합니다. 사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.

* **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**

* `Install-Package Microsoft.EntityFrameworkCore.SqlServer` 실행

다음 단계에서는 일부 Entity Framework Tools를 사용하여 데이터베이스를 리버스 엔지니어링합니다. 따라서 도구 패키지도 설치합니다.

* `Install-Package Microsoft.EntityFrameworkCore.Tools` 실행

## <a name="reverse-engineer-the-model"></a>모델 리버스 엔지니어링

이제 기존 데이터베이스를 기반으로 EF 모델을 만듭니다.

* **도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔**

* 다음 명령을 실행하여 기존 데이터베이스에서 모델을 만듭니다.

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> 위의 명령에 `-Tables` 인수를 추가하여 엔터티를 생성할 테이블을 지정할 수 있습니다. 예를 들어, `-Tables Blog,Post`을 입력합니다.

리버스 엔지니어링 프로세스에서는 기존 데이터베이스의 스키마를 기반으로 엔터티 클래스(`Blog` 및 `Post`) 및 파생 컨텍스트(`BloggingContext`)를 만들었습니다.

엔터티 클래스는 쿼리하고 저장할 데이터를 나타내는 단순 C# 개체입니다. `Blog` 및 `Post` 엔터티 클래스는 다음과 같습니다.

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> 지연 로드를 사용하도록 설정하려면 탐색 속성을 `virtual`로 만들 수 있습니다(Blog.Post 및 Post.Blog).

컨텍스트는 데이터베이스를 포함한 세션을 나타냅니다. 엔터티 클래스의 인스턴스를 쿼리하고 저장하는 데 사용할 수 있는 메서드가 있습니다.

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a>모델 사용

이제 모델을 사용하여 데이터 액세스를 수행할 수 있습니다.

* *Program.cs*를 엽니다.

* 파일의 내용을 다음 코드로 바꿉니다.

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* 디버그 > 디버깅하지 않고 시작

  하나의 블로그가 데이터베이스에 저장되고 모든 블로그의 세부 정보가 콘솔에 출력되는 것을 알 수 있습니다.

  ![이미지](_static/output-existing-db.png)

## <a name="additional-resources"></a>추가 리소스

* [새 데이터베이스를 포함한 .NET Framework의 EF Core](xref:core/get-started/full-dotnet/new-db)
* [새 데이터베이스를 포함한 .NET Core의 EF Core - SQLite](xref:core/get-started/netcore/new-db-sqlite) - 플랫폼 간 콘솔 EF 자습서.
