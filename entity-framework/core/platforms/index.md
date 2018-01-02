---
title: "지원 되는 .NET 구현 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 6b6ea9ca833810169d0d850f3bea8776b761c007
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="net-implementations-supported-by-ef-core"></a>EF Core에서 지원되는 .NET 구현

.NET 코드를 작성할 수 있으면 어디서나 EF Core를 사용할 수 있게 하기 위해 노력하고 있지만 아직 노력이 더 필요합니다. 다음 표에서는 EF Core를 사용할 수 있게 하고자 하는 각 .NET 구현에 대한 지침을 제공합니다.

EF Core 2.0은 [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard)을 대상으로 하므로 이를 지원하는 .NET 구현이 필요합니다.

| .NET 구현 | 상태 | 1.x 필요 | 2.x 필요
|-|-|-|-
| **.NET Core**([ASP.NET Core](../get-started/aspnetcore/index.md), [Console](../get-started/netcore/index.md) 등) | **완전히 지원 및 권장:** 자동 테스트를 거쳤으며 여러 응용 프로그래에서 성공적으로 사용된 것으로 알려져 있습니다. | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/) | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)
| **.NET Framework**(WinForms, WPF, ASP.NET, [Console](../get-started/full-dotnet/index.md) 등) | **완전히 지원 및 권장:** 자동 테스트를 거쳤으며 여러 응용 프로그래에서 성공적으로 사용된 것으로 알려져 있습니다. EF 6도 이 플랫폼에서 사용할 수 있습니다(올바른 기술을 선택하려면 [EF Core & EF6 비교](../../efcore-and-ef6/index.md) 참조). | .NET Framework 4.5.1 | .NET Framework 4.6.1
| **Mono & Xamarin** | **진행 중 - 문제가 발생할 수 있음:** EF Core 팀 및 고객에서 부분적으로 테스트했습니다. 얼리어답터가 성공 사례를 보고했으나 [문제가 있었고](https://github.com/aspnet/entityframework/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) 테스트가 진행되면서 다른 문제가 나타날 수 있습니다. 특히, Xamarin.iOS에서 EF Core 2.0을 통해 개발된 일부 응용 프로그램이 제대로 작동하지 않을 수 있는 제한이 있습니다. | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7 | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5
| [**유니버설 Windows 플랫폼**](../get-started/uwp/index.md) | **진행 중 - 문제가 발생할 수 있음:** EF Core 팀 및 고객에서 부분적으로 테스트했습니다. 릴리스 빌드에서 일반적으로 사용되며 Windows Store 배포를 위한 요규 사항인 .NET Native 툴체인으로 컴파일할 때 [문제](https://github.com/aspnet/entityframework/issues?utf8=%E2%9C%93&q=is%3Aopen%20is%3Aissue%20label%3Aarea-uwp%20)가 보고되었습니다. .NET Native를 사용하지 않거나 테스트만 해보려는 경우 이 문제는 대부분 영향이 없습니다. | [최신 .NET UWP 5 패키지](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/5.4.1) | [최신 .NET UWP 6 패키지](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) <sup>(1)</sup>

<sup>(1) </sup> 이 버전의 .NET UWP는 .NET Standard 2.0에 대한 지원을 추가하고 .NET Native 2.0을 포함하여 이전에 보고된 대부분의 호환성 문제를 해결하지만 EF Core 2.0에서의 [몇 가지 다른 문제](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A2.0.1+label%3Aarea-uwp)는 향후 패치 릴리스를 통해 해결될 예정입니다.

예상대로 작동하지 않는 다른 조합의 경우 [EF Core 문제 추적기](https://github.com/aspnet/entityframeworkcore/issues/new)에 새 문제를 작성해 주시기 바랍니다. Xamarin 관련 문제는 [Xamarin 문제 추적기](https://bugzilla.xamarin.com/newbug)를 사용하세요.
