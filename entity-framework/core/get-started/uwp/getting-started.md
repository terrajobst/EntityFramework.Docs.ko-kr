---
title: "UWP에서 시작 - 새 데이터베이스 - EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/01/2017
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>UWP(유니버설 Windows 플랫폼)에서 새 데이터베이스로 EF Core 시작

> [!NOTE]
> 이 자습서에서는 EF Core 2.0.1(ASP.NET Core 및 .NET Core SDK 2.0.3과 함께 릴리스됨)을 사용합니다. EF Core 2.0.0에는 UWP 환경을 조성하는 데 필요한 일부 중요한 버그 수정이 없습니다.

이 연습에서는 Entity Framework를 사용하여 로컬 SQLite 데이터베이스에 대해 기본 데이터 액세스를 수행하는 UWP(유니버설 Windows 플랫폼) 응용 프로그램을 빌드합니다.

> [!IMPORTANT]
> **UWP의 LINQ 쿼리에서는 익명 형식을 사용하지 않는 것이 좋습니다**. UWP 응용 프로그램을 앱 스토어에 배포하려면 .NET 네이티브로 응용 프로그램을 컴파일해야 합니다. 익명 형식의 쿼리는 .NET 네이티브에서 성능이 저하됩니다.

> [!TIP]
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite)을 볼 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 연습을 완료하려면 다음 항목이 필요합니다.

* [Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10)(10.0.16299.0)

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 이상.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 버전 15.4 이상(**유니버설 Windows 플랫폼 개발** 워크로드 포함).

## <a name="create-a-new-model-project"></a>새 모델 프로젝트 만들기

> [!WARNING]
> .NET Core 도구가 UWP 프로젝트와 상호 작용하는 방법의 제한 사항으로 인해 패키지 관리자 콘솔에서 마이그레이션 명령을 실행할 수 있으려면 모델을 UWP 이외 프로젝트에 배치해야 합니다.

* Visual Studio를 엽니다.

* 파일 > 새로 만들기 > 프로젝트...

* 왼쪽 메뉴에서 [템플릿] > [Visual C#]을 선택합니다.

* **클래스 라이브러리(.NET Standard)** 프로젝트 템플릿을 선택합니다.

* 프로젝트에 이름을 지정하고 **확인**을 클릭합니다.

## <a name="install-entity-framework"></a>Entity Framework 설치

EF Core 를 사용하려면 대상으로 지정할 데이터베이스 공급자에 대한 패키지를 설치합니다. 이 연습에서는 SQLite를 사용합니다. 사용 가능한 공급자 목록은 [데이터베이스 공급자](../../providers/index.md)를 참조하세요.

* 도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔

* `Install-Package Microsoft.EntityFrameworkCore.Sqlite` 실행

이 연습의 뒷부분에서는 일부 Entity Framework Tools를 사용하여 데이터베이스를 유지 관리합니다. 따라서 도구 패키지도 설치합니다.

* `Install-Package Microsoft.EntityFrameworkCore.Tools` 실행

* .csproj 파일을 편집하고 `<TargetFramework>netstandard2.0</TargetFramework>`를 `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`로 바꿉니다.

## <a name="create-your-model"></a>모델 만들기

이제 모델을 구성하는 컨텍스트 및 엔터티 클래스를 정의할 수 있습니다.

* 프로젝트 > 클래스 추가...

* 이름으로 *Model.cs*를 입력하고 **확인**을 클릭합니다.

* 파일의 내용을 다음 코드로 바꿉니다.

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>새 UWP 프로젝트 만들기

* Visual Studio를 엽니다.

* 파일 > 새로 만들기 > 프로젝트...

* 왼쪽 메뉴에서 [템플릿] > [Visual C#] > [Windows 유니버설]을 선택합니다.

* **빈 앱(유니버설 Windows)** 프로젝트 템플릿을 선택합니다.

* 프로젝트에 이름을 지정하고 **확인**을 클릭합니다.

* 대상 및 최소 버전을 `Windows 10 Fall Creators Update (10.0; build 16299.0)` 이상으로 설정합니다.

## <a name="create-your-database"></a>데이터베이스 만들기

이제 모델이 있으므로 마이그레이션을 사용하여 데이터베이스를 만들 수 있습니다.

* 도구 -> NuGet 패키지 관리자 -> 패키지 관리자 콘솔

* 모델 프로젝트를 기본 프로젝트로 선택하고 시작 프로젝트로 설정합니다.

* `Add-Migration MyFirstMigration`를 실행하여 마이그레이션을 스캐폴딩하고 모델에 대한 초기 테이블 집합을 만듭니다.

앱이 실행되는 장치에서 데이터베이스를 만들기 위해 일부 코드를 추가하여 응용 프로그램 시작 시 로컬 데이터베이스에 보류 중인 마이그레이션을 적용합니다. 이렇게 하면 앱이 처음 실행될 때 로컬 데이터베이스가 만들어집니다.

* **솔루션 탐색기**에서 **App.xaml**을 마우스 오른쪽 단추로 클릭하고 **코드 보기**를 선택합니다.

* 파일 시작 부분에 강조 표시된 using을 추가합니다.

* 강조 표시된 코드를 추가하여 보류 중인 마이그레이션을 적용합니다.

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> 나중에 모델을 변경할 경우 `Add-Migration` 명령을 사용하여 새 마이그레이션을 스캐폴딩하고 데이터베이스에 해당 변경 내용을 적용합니다. 보류 중인 마이그레이션은 응용 프로그램이 시작될 때 각 장치의 로컬 데이터베이스에 적용됩니다.
>
>EF는 데이터베이스의 `__EFMigrationsHistory` 테이블을 사용하여 이미 데이터베이스에 적용된 마이그레이션을 추적합니다.

## <a name="use-your-model"></a>모델 사용

이제 모델을 사용하여 데이터 액세스를 수행할 수 있습니다.

* *MainPage.xaml*을 엽니다.

* 아래에 강조 표시된 페이지 로드 처리기 및 UI 콘텐츠를 추가합니다.

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

이제 코드를 추가하여 데이터베이스와 UI를 연결합니다.

* **솔루션 탐색기**에서 **MainPage.xaml**을 마우스 오른쪽 단추로 클릭하고 **코드 보기**를 선택합니다.

* 다음 목록에서 강조 표시된 코드를 추가합니다.

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

이제 응용 프로그램을 실행하여 작동하는지 확인할 수 있습니다.

* 디버그 > 디버깅하지 않고 시작

* 응용 프로그램이 빌드되고 시작됩니다.

* URL을 입력하고 **추가** 단추를 클릭합니다.

![image](_static/create.png)

![image](_static/list.png)

## <a name="next-steps"></a>다음 단계

> [!TIP]
> 엔터티 형식에서 [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx)를 구현하고 `ChangeTrackingStrategy.ChangingAndChangedNotifications`를 사용하여 `SaveChanges()` 성능을 개선할 수 있습니다.

짜잔! 이제 Entity Framework를 실행하는 간단한 UWP 앱이 있습니다.

Entity Framework의 기능에 대해 자세히 알아보려면 이 문서의 다른 문서를 확인하세요.
