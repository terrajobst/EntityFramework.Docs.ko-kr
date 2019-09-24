---
title: 지원 되는 .NET 구현 - EF Core
author: rowanmiller
ms.date: 08/30/2017
uid: core/platforms/index
ms.openlocfilehash: ac3cf3d0a84200bbf4ba7ec18b9115e06d1748f4
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149213"
---
# <a name="net-implementations-supported-by-ef-core"></a>EF Core에서 지원되는 .NET 구현

개발자가 모든 최신 .NET 구현에서 EF Core를 사용할 수 있도록 하기 위해 계속 노력하고 있습니다. .NET Core에서의 EF Core 지원은 널리 사용되는 자동화된 테스트 및 여러 애플리케이션에서 다루어지는 반면, Mono, Xamarin 및 UWP에는 몇 가지 문제가 있습니다.

## <a name="overview"></a>개요

다음 표에서는 각 .NET 구현에 대한 지침을 제공합니다.

| EF Core                       | 1.x    | 2.x        | 3.x             |
|:------------------------------|:-------|:-----------|:----------------|
| .NET Standard                 | 1.3    | 2.0        | 2.1             |
| .NET Core                     | 1.0    | 2.0        | 3.0             |
| .NET Framework<sup>(1)</sup>  | 4.5.1  | 4.7.2      | (지원되지 않음) |
| Mono                          | 4.6    | 5.4        | 6.4             |
| Xamarin.iOS<sup>(2)</sup>     | 10.0   | 10.14      | 12.16           |
| Xamarin.Android<sup>(2)</sup> | 7.0    | 8.0        | 10.0            |
| UWP<sup>(3)</sup>             | 10.0   | 10.0.16299 | TBD             |
| Unity<sup>(4)</sup>           | 2018.1 | 2018.1     | TBD             |

<sup>(1)</sup> 아래 [.NET Framework](#net-framework) 섹션을 참조하세요.

<sup>(2)</sup> Xamarin에는 EF Core를 사용하여 개발된 일부 애플리케이션이 제대로 작동하지 않을 수 있는 문제 및 알려진 제한이 있습니다. 해결 방법은 [활성 문제](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) 목록을 확인하세요.

<sup>(3)</sup> EF Core 2.0.1 이상을 권장합니다. [.NET Core UWP 6.x 패키지](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/)를 설치합니다. 이 문서의 [유니버설 Windows 플랫폼](#universal-windows-platform) 섹션을 참조하세요.

<sup>(4)</sup> Unity에는 문제 및 알려진 제한 사항이 있습니다. [미해결 문제점](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity) 목록을 확인하세요.

## <a name="net-framework"></a>.NET Framework

.NET Framework를 대상으로 하는 애플리케이션은 .NET Standard 라이브러리와 함께 작동하도록 변경해야 할 수 있습니다.

프로젝트 파일을 편집하고 최초 속성 그룹에 다음 항목이 표시되는지 확인합니다.

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

테스트 프로젝트의 경우 다음 항목도 있는지 확인합니다.

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

이전 버전의 Visual Studio를 사용하려면 [NuGet 클라이언트를 버전 3.6.0으로 업그레이드](https://www.nuget.org/downloads)하여 .NET Standard 2.0 라이브러리와 함께 작동하도록 합니다.

또한 가능한 경우 NuGet packages.config에서 PackageReference로 마이그레이션하는 것이 좋습니다. 프로젝트 파일에 다음 속성을 추가합니다.

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a>UWP

이전 버전의 EF Core 및 .NET UWP에는 여러 가지 호환성 문제가 있었습니다(특히 .NET 네이티브 도구 체인을 사용하여 컴파일된 애플리케이션 관련). 새 .NET UWP 버전은 .NET Standard 2.0에 대한 지원을 추가하고 .NET 네이티브 2.0을 포함하여 이전에 보고된 호환성 문제 대부분을 해결했습니다. EF Core 2.0.1은 UWP와 함께 완벽하게 테스트되었지만 테스트가 자동화되지 않았습니다.

UWP에서 EF Core를 사용하는 경우:

* 쿼리 성능을 최적화하려면 LINQ 쿼리에서 무명 형식을 사용하지 않습니다. UWP 애플리케이션을 앱 스토어에 배포하려면 .NET 네이티브로 애플리케이션을 컴파일해야 합니다. 익명 형식의 쿼리는 .NET 네이티브에서 성능이 저하됩니다.

* `SaveChanges()` 성능을 최적화하려면 [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy)를 사용하고 엔터티 형식에서 [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx) 및 [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx)를 구현하세요.

## <a name="report-issues"></a>문제 보고

예상대로 작동하지 않는 다른 조합의 경우 [EF Core 문제 추적기](https://github.com/aspnet/entityframeworkcore/issues/new)에 새 문제를 작성해 주시기 바랍니다. Xamarin 관련 문제는 [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) 또는 [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new)의 문제 추적기를 사용하세요.
