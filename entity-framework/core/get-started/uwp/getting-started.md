---
title: UWP에서 시작 - 새 데이터베이스 - EF 코어
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 456967a0dc981053919064a19cc9c98bf7309865
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022378"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>UWP(유니버설 Windows 플랫폼)에서 새 데이터베이스로 EF Core 시작

이 자습서에서는 Entity Framework Core를 사용하여 로컬 SQLite 데이터베이스에 대한 기본 데이터 액세스를 수행하는 UWP(유니버설 Windows 플랫폼) 응용 프로그램을 빌드합니다.

[GitHub에서 이 아티클의 샘플을 봅니다](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>전제 조건

* [Windows 10 Fall Creators Update(10.0; 빌드 16299) 이상](https://support.microsoft.com/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 버전 15.7 이상](https://www.visualstudio.com/downloads/)(**유니버설 Windows 플랫폼 개발** 워크로드 포함).

* [.NET Core 2.1 SDK 이상](https://www.microsoft.com/net/core) 또는 이후.

> [!IMPORTANT]
> 이 자습서에서는 Entity Framework Core [마이그레이션](xref:core/managing-schemas/migrations/index) 명령을 사용하여 데이터베이스의 스키마를 작성하고 업데이트합니다.
> 이러한 명령은 UWP 프로젝트에 작동하지 않습니다.
> 이러한 이유로 응용 프로그램의 데이터 모델이 공유 라이브러리 프로젝트에 배치되고 별도의 .NET Core 콘솔 응용 프로그램이 명령을 실행하는 데 사용됩니다.

## <a name="create-a-library-project-to-hold-the-data-model"></a>데이터 모델을 보관할 라이브러리 프로젝트 만들기

* Visual Studio를 엽니다.

* **파일 > 새로 만들기 > 프로젝트**

* 왼쪽 메뉴에서 **설치됨 > Visual C# > .NET Standard**를 선택합니다.

* **클래스 라이브러리(.NET Standard)** 템플릿을 선택합니다.

* 프로젝트 이름을 *Blogging.Model*로 지정합니다.

* 솔루션의 이름을 *Blogging*으로 지정합니다.

* **확인**을 클릭합니다.

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a>데이터 모델 프로젝트에서 Entity Framework Core 런타임 설치

EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다. 이 자습서에서는 SQLite를 사용합니다. 사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.

* **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**.

* 라이브러리 프로젝트 *Blogging.Model*이 패키지 관리자 콘솔에서 **기본 프로젝트**로 선택되었는지 확인합니다.

* `Install-Package Microsoft.EntityFrameworkCore.Sqlite` 실행

## <a name="create-the-data-model"></a>데이터 모델 만들기

이제 모델을 구성하는 *DbContext* 및 엔터티 클래스를 정의할 수 있습니다.

* *Class1.cs*를 삭제합니다.

* 다음 코드로 *Model.cs*를 만듭니다.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a>마이그레이션 명령을 실행할 새 콘솔 프로젝트 만들기

* **솔루션 탐색기**에서 솔루션을 마우스 오른쪽 단추로 클릭한 다음, **추가 > 새 프로젝트**를 선택합니다.

* 왼쪽 메뉴에서 **설치됨 Visual C# > .NET Core**를 선택합니다.

* **콘솔 앱(.NET Core)** 프로젝트 템플릿을 선택합니다.

* *Blogging.Migrations.Startup* 프로젝트 이름을 지정하고 **확인**을 클릭합니다.

* *Blogging.Migrations.Startup* 프로젝트의 프로젝트 참조를 *Blogging.Model* 프로젝트에 추가합니다.

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a>마이그레이션 시작 프로젝트에 Entity Framework Core 도구 설치

패키지 관리자 콘솔에서 EF Core 마이그레이션 명령을 사용하려면 콘솔 응용 프로그램에서 EF Core 도구 패키지를 설치하세요.

* **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**

* `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup` 실행

## <a name="create-the-initial-migration"></a>초기 마이그레이션 만들기

 시작 프로젝트로 콘솔 응용 프로그램을 지정하여 초기 마이그레이션을 만드세요.

* `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup` 실행

이 명령은 데이터 모델에 대한 초기 데이터베이스 테이블 집합을 만드는 마이그레이션을 스캐폴딩합니다.

## <a name="create-the-uwp-project"></a>UWP 프로젝트 만들기

* **솔루션 탐색기**에서 솔루션을 마우스 오른쪽 단추로 클릭한 다음, **추가 > 새 프로젝트**를 선택합니다.

* 왼쪽 메뉴에서 **설치됨 > Visual C# > Windows 유니버설**을 선택합니다.

* **빈 앱(유니버설 Windows)** 프로젝트 템플릿을 선택합니다.

* *Blogging.UWP* 프로젝트 이름을 지정하고 **확인**을 클릭합니다.

> [!IMPORTANT]
> 대상 및 최소 버전을 **Windows 10 Fall Creators Update(10.0; 빌드 16299.0)** 이상으로 설정합니다.
> 이전 버전의 Windows 10에서는 Entity Framework Core에 필요한 .NET Standard 2.0을 지원하지 않습니다.

## <a name="add-code-to-create-the-database-on-application-startup"></a>응용 프로그램 시작 시 데이터베이스를 만드는 코드 추가

앱이 실행되는 장치에서 데이터베이스를 만들려면 응용 프로그램 시작 시 로컬 데이터베이스에 보류 중인 마이그레이션을 적용할 코드를 추가합니다. 이렇게 하면 앱이 처음 실행될 때 로컬 데이터베이스가 만들어집니다.

* *Blogging.UWP* 프로젝트의 프로젝트 참조를 *Blogging.Model* 프로젝트에 추가합니다.

* *App.xaml.cs*를 엽니다.

* 강조 표시된 코드를 추가하여 보류 중인 마이그레이션을 적용합니다.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> 모델을 변경하는 경우 `Add-Migration` 명령을 사용하여 새 마이그레이션을 스캐폴딩하고 데이터베이스에 해당 변경 내용을 적용합니다. 보류 중인 마이그레이션은 응용 프로그램이 시작될 때 각 장치의 로컬 데이터베이스에 적용됩니다.
>
>EF Core는 데이터베이스의 `__EFMigrationsHistory` 테이블을 사용하여 이미 데이터베이스에 적용된 마이그레이션을 추적합니다.

## <a name="use-the-data-model"></a>데이터 모델 사용

이제 EF Core를 사용하여 데이터 액세스를 수행할 수 있습니다.

* *MainPage.xaml*을 엽니다.

* 아래에 강조 표시된 페이지 로드 처리기 및 UI 콘텐츠를 추가합니다.

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

이제 코드를 추가하여 데이터베이스와 UI 연결합니다.

* *MainPage.xaml.cs*를 엽니다.

* 다음 목록에서 강조 표시된 코드를 추가합니다.

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

이제 응용 프로그램을 실행하여 작동하는지 확인할 수 있습니다.

* **솔루션 탐색기**에서 *Blogging.UWP* 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **배포**를 선택합니다.

* *Blogging.UWP*를 시작 프로젝트로 설정합니다.

* **디버그 > 디버깅하지 않고 시작**

  앱을 빌드하고 실행합니다.

* URL을 입력하고 **추가** 단추를 클릭합니다.

  ![이미지](_static/create.png)

  ![이미지](_static/list.png)

  짜잔! 이제 Entity Framework Core를 실행하는 간단한 UWP 앱이 있습니다.

## <a name="next-steps"></a>다음 단계

UWP와 함께 EF Core를 사용할 때 알아야 할 호환성 및 성능 정보는 [EF Core에서 지원하는 .NET 구현](../../platforms/index.md#universal-windows-platform)을 참조하세요.

Entity Framework Core 기능에 대해 자세히 알아보려면 이 설명서의 다른 문서를 확인하세요.
