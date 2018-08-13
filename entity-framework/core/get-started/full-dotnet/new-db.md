---
title: .NET Framework에서 시작 - 새 데이터베이스 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 088ac915041489242eb8090e7bf3a2bdc8036534
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614430"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>.NET Framework에서 새 데이터베이스로 EF Core 시작

이 자습서에서는 Entity Framework를 사용하여 Microsoft SQL Server 데이터베이스에 대해 기본 데이터 액세스를 수행하는 콘솔 응용 프로그램을 빌드합니다. 마이그레이션을 사용하여 모델에서 데이터베이스를 만듭니다.

[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).

## <a name="prerequisites"></a>전제 조건

* [Visual Studio 2017 버전 15.7 이상](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a>새 프로젝트 만들기

* Visual Studio 2017을 엽니다.

* **파일 > 새로 만들기 > 프로젝트...**

* 왼쪽 메뉴에서 **설치됨 > Visual C# > Windows Desktop**을 선택합니다.

* **콘솔 앱(.NET Framework)** 프로젝트 템플릿을 선택합니다.

* 프로젝트가 **.NET Framework 4.6.1** 이상을 대상으로 지정하도록 합니다.

* 프로젝트 이름을 *ConsoleApp.NewDb*로 지정하고 **확인**을 클릭합니다.

## <a name="install-entity-framework"></a>Entity Framework 설치

EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다. 이 자습서에서는 SQL Server를 사용합니다. 사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.

* 도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔

* `Install-Package Microsoft.EntityFrameworkCore.SqlServer` 실행

이 자습서의 뒷부분에서는 일부 Entity Framework Tools를 사용하여 데이터베이스를 유지 관리합니다. 따라서 도구 패키지도 설치합니다.

* `Install-Package Microsoft.EntityFrameworkCore.Tools` 실행

## <a name="create-the-model"></a>모델 만들기

이제 모델을 구성하는 컨텍스트 및 엔터티 클래스를 정의할 수 있습니다.

* **프로젝트 > 클래스 추가...**

* 이름으로 *Model.cs*를 입력하고 **확인**을 클릭합니다.

* 파일의 내용을 다음 코드로 바꿉니다.

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> 실제 응용 프로그램에서는 각 클래스를 별도의 파일에 저장하고 구성 파일 또는 환경 변수에 연결 문자열을 저장합니다. 편의상 모든 항목은 이 자습서의 단일 코드 파일에 위치합니다.

## <a name="create-the-database"></a>데이터베이스 만들기

이제 모델이 있으므로 마이그레이션을 사용하여 데이터베이스를 만들 수 있습니다.

* **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**

* `Add-Migration InitialCreate`를 실행하여 마이그레이션을 스캐폴딩하고 모델에 대한 초기 테이블 집합을 만듭니다.

* `Update-Database`를 실행하여 새 마이그레이션을 데이터베이스에 적용합니다. 데이터베이스가 아직 없기 때문에 마이그레이션이 적용되기 전에 데이터베이스가 생성됩니다.

> [!TIP]  
> 모델을 변경할 경우 `Add-Migration` 명령을 사용하여 새 마이그레이션을 스캐폴딩하고 데이터베이스에서 해당 스키마를 변경합니다. 스캐폴딩된 코드를 확인하고 필요한 내용을 변경한 후 `Update-Database` 명령을 사용하여 변경 내용을 데이터베이스에 적용할 수 있습니다.
>
> EF는 데이터베이스의 `__EFMigrationsHistory` 테이블을 사용하여 이미 데이터베이스에 적용된 마이그레이션을 추적합니다.

## <a name="use-the-model"></a>모델 사용

이제 모델을 사용하여 데이터 액세스를 수행할 수 있습니다.

* *Program.cs*를 엽니다.

* 파일의 내용을 다음 코드로 바꿉니다.

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* **디버그 > 디버깅하지 않고 시작**

  하나의 블로그가 데이터베이스에 저장되고 모든 블로그의 세부 정보가 콘솔에 출력되는 것을 알 수 있습니다.

  ![이미지](_static/output-new-db.png)

## <a name="additional-resources"></a>추가 리소스

* [기존 데이터베이스를 포함한 .NET Framework의 EF Core](xref:core/get-started/full-dotnet/existing-db)
* [새 데이터베이스를 포함한 .NET Core의 EF Core - SQLite](xref:core/get-started/netcore/new-db-sqlite) - 플랫폼 간 콘솔 EF 자습서.
