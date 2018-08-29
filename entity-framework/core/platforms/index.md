---
title: 지원 되는 .NET 구현 - EF Core
author: rowanmiller
ms.date: 08/30/2017
uid: core/platforms/index
ms.openlocfilehash: 347965818f0eab9a86411f66eaaf10cb3aa8d652
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996440"
---
# <a name="net-implementations-supported-by-ef-core"></a>EF Core에서 지원되는 .NET 구현

.NET 코드를 작성할 수 있으면 어디서나 EF Core를 사용할 수 있게 하기 위해 노력하고 있지만 아직 노력이 더 필요합니다. .NET Core와 .NET Framework에서 EF Core 지원이 널리 사용되는 자동화된 테스트 및 여러 응용 프로그램에서 다루어지는 반면, Mono, Xamarin 및 UWP에는 몇 가지 문제가 있습니다.

## <a name="overview"></a>개요

다음 표에서는 각 .NET 구현에 대한 지침을 제공합니다.

| .NET 구현                                                                                                  | 상태                                                             | EF Core 1.x 요구 사항                                                                                | EF Core 2.x 요구 사항<sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **.NET Core**([ASP.NET Core](../get-started/aspnetcore/index.md), [Console](../get-started/netcore/index.md) 등) | 완벽하게 지원 및 권장됨                                    | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/)                                                | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)                                                |
| **.NET Framework**(WinForms, WPF, ASP.NET, [Console](../get-started/full-dotnet/index.md) 등)                    | 완벽하게 지원 및 권장됨 EF6도 지원<sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Mono & Xamarin**                                                                                                   | 진행 중<sup>(3)</sup>                                         | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**유니버설 Windows 플랫폼**](../get-started/uwp/index.md)                                                        | EF Core 2.0.1 권장됨<sup>(4)</sup>                           | [.NET Core UWP 5.x 패키지](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [.NET Core UWP 6.x 패키지](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1)</sup>EF Core 2.0은 [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard)을 대상으로 하므로 이를 지원하는 .NET 구현이 필요합니다.

<sup>(2)</sup>올바른 기술을 선택하려면 [EF Core 및 EF6 비교](../../efcore-and-ef6/index.md)를 참조하세요.

<sup>(3)</sup>Xamarin에는 EF Core 2.0을 사용하여 개발된 일부 응용 프로그램이 제대로 작동하지 않을 수 있는 문제 및 알려진 제한이 있습니다. 해결 방법은 [활성 문제](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) 목록을 확인하세요.

<sup>(4)</sup> 이 문서의 [유니버설 Windows 플랫폼](#universal-windows-platform) 섹션을 참조하세요.

## <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

이전 버전의 EF Core 및 .NET UWP에는 여러 가지 호환성 문제가 있었습니다(특히 .NET 네이티브 도구 체인을 사용하여 컴파일된 응용 프로그램 관련). 새 .NET UWP 버전은 .NET Standard 2.0에 대한 지원을 추가하고 .NET 네이티브 2.0을 포함하여 이전에 보고된 호환성 문제 대부분을 해결했습니다. EF Core 2.0.1은 UWP와 함께 완벽하게 테스트되었지만 테스트가 자동화되지 않았습니다.

UWP에서 EF Core를 사용하는 경우:

* 쿼리 성능을 최적화하려면 LINQ 쿼리에서 무명 형식을 사용하지 않습니다. UWP 응용 프로그램을 앱 스토어에 배포하려면 .NET 네이티브로 응용 프로그램을 컴파일해야 합니다. 익명 형식의 쿼리는 .NET 네이티브에서 성능이 저하됩니다.

* `SaveChanges()` 성능을 최적화하려면 [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy)를 사용하고 엔터티 형식에서 [INotifyPropertyChanged](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx) 및 [INotifyCollectionChanged](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx)를 구현하세요.

## <a name="report-issues"></a>문제 보고

예상대로 작동하지 않는 다른 조합의 경우 [EF Core 문제 추적기](https://github.com/aspnet/entityframeworkcore/issues/new)에 새 문제를 작성해 주시기 바랍니다. Xamarin 관련 문제는 [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) 또는 [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new)의 문제 추적기를 사용하세요.
