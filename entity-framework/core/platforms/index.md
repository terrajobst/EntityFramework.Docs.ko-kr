---
title: 지원 되는 .NET 구현 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 790628c407cc4374fee4ebde8201783955afdcc3
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900332"
---
# <a name="net-implementations-supported-by-ef-core"></a>EF Core에서 지원되는 .NET 구현

.NET 코드를 작성할 수 있으면 어디서나 EF Core를 사용할 수 있게 하기 위해 노력하고 있지만 아직 노력이 더 필요합니다. .NET Core와 .NET Framework에서 EF Core 지원이 널리 사용되는 자동화된 테스트 및 여러 응용 프로그램에서 다루어지는 반면, Mono, Xamarin 및 UWP에는 몇 가지 문제가 있습니다.

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

<sup>(4)</sup>이전 버전의 EF Core 및 .NET UWP에는 여러 가지 호환성 문제가 있었습니다(특히 .NET 네이티브 도구 체인을 사용하여 컴파일된 응용 프로그램 관련). 새 .NET UWP 버전은 .NET Standard 2.0에 대한 지원을 추가하고 .NET 네이티브 2.0을 포함하여 이전에 보고된 호환성 문제 대부분을 해결했습니다. EF Core 2.0.1은 UWP와 함께 완벽하게 테스트되었지만 테스트가 자동화되지 않았습니다.

예상대로 작동하지 않는 다른 조합의 경우 [EF Core 문제 추적기](https://github.com/aspnet/entityframeworkcore/issues/new)에 새 문제를 작성해 주시기 바랍니다. Xamarin 관련 문제는 [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) 또는 [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new)의 문제 추적기를 사용하세요.
