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
# <a name="net-implementations-supported-by-ef-core"></a><span data-ttu-id="db62a-102">EF Core에서 지원되는 .NET 구현</span><span class="sxs-lookup"><span data-stu-id="db62a-102">.NET implementations supported by EF Core</span></span>

<span data-ttu-id="db62a-103">개발자가 모든 최신 .NET 구현에서 EF Core를 사용할 수 있도록 하기 위해 계속 노력하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-103">We want EF Core to be available to developers on all modern .NET implementations, and we're still working towards that goal.</span></span> <span data-ttu-id="db62a-104">.NET Core에서의 EF Core 지원은 널리 사용되는 자동화된 테스트 및 여러 애플리케이션에서 다루어지는 반면, Mono, Xamarin 및 UWP에는 몇 가지 문제가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-104">While EF Core's support on .NET Core is covered by automated testing and many applications known to be using it successfully, Mono, Xamarin and UWP have some issues.</span></span>

## <a name="overview"></a><span data-ttu-id="db62a-105">개요</span><span class="sxs-lookup"><span data-stu-id="db62a-105">Overview</span></span>

<span data-ttu-id="db62a-106">다음 표에서는 각 .NET 구현에 대한 지침을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-106">The following table provides guidance for each .NET implementation:</span></span>

| <span data-ttu-id="db62a-107">EF Core</span><span class="sxs-lookup"><span data-stu-id="db62a-107">EF Core</span></span>                       | <span data-ttu-id="db62a-108">1.x</span><span class="sxs-lookup"><span data-stu-id="db62a-108">1.x</span></span>    | <span data-ttu-id="db62a-109">2.x</span><span class="sxs-lookup"><span data-stu-id="db62a-109">2.x</span></span>        | <span data-ttu-id="db62a-110">3.x</span><span class="sxs-lookup"><span data-stu-id="db62a-110">3.x</span></span>             |
|:------------------------------|:-------|:-----------|:----------------|
| <span data-ttu-id="db62a-111">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="db62a-111">.NET Standard</span></span>                 | <span data-ttu-id="db62a-112">1.3</span><span class="sxs-lookup"><span data-stu-id="db62a-112">1.3</span></span>    | <span data-ttu-id="db62a-113">2.0</span><span class="sxs-lookup"><span data-stu-id="db62a-113">2.0</span></span>        | <span data-ttu-id="db62a-114">2.1</span><span class="sxs-lookup"><span data-stu-id="db62a-114">2.1</span></span>             |
| <span data-ttu-id="db62a-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="db62a-115">.NET Core</span></span>                     | <span data-ttu-id="db62a-116">1.0</span><span class="sxs-lookup"><span data-stu-id="db62a-116">1.0</span></span>    | <span data-ttu-id="db62a-117">2.0</span><span class="sxs-lookup"><span data-stu-id="db62a-117">2.0</span></span>        | <span data-ttu-id="db62a-118">3.0</span><span class="sxs-lookup"><span data-stu-id="db62a-118">3.0</span></span>             |
| <span data-ttu-id="db62a-119">.NET Framework<sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="db62a-119">.NET Framework<sup>(1)</sup></span></span>  | <span data-ttu-id="db62a-120">4.5.1</span><span class="sxs-lookup"><span data-stu-id="db62a-120">4.5.1</span></span>  | <span data-ttu-id="db62a-121">4.7.2</span><span class="sxs-lookup"><span data-stu-id="db62a-121">4.7.2</span></span>      | <span data-ttu-id="db62a-122">(지원되지 않음)</span><span class="sxs-lookup"><span data-stu-id="db62a-122">(not supported)</span></span> |
| <span data-ttu-id="db62a-123">Mono</span><span class="sxs-lookup"><span data-stu-id="db62a-123">Mono</span></span>                          | <span data-ttu-id="db62a-124">4.6</span><span class="sxs-lookup"><span data-stu-id="db62a-124">4.6</span></span>    | <span data-ttu-id="db62a-125">5.4</span><span class="sxs-lookup"><span data-stu-id="db62a-125">5.4</span></span>        | <span data-ttu-id="db62a-126">6.4</span><span class="sxs-lookup"><span data-stu-id="db62a-126">6.4</span></span>             |
| <span data-ttu-id="db62a-127">Xamarin.iOS<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="db62a-127">Xamarin.iOS<sup>(2)</sup></span></span>     | <span data-ttu-id="db62a-128">10.0</span><span class="sxs-lookup"><span data-stu-id="db62a-128">10.0</span></span>   | <span data-ttu-id="db62a-129">10.14</span><span class="sxs-lookup"><span data-stu-id="db62a-129">10.14</span></span>      | <span data-ttu-id="db62a-130">12.16</span><span class="sxs-lookup"><span data-stu-id="db62a-130">12.16</span></span>           |
| <span data-ttu-id="db62a-131">Xamarin.Android<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="db62a-131">Xamarin.Android<sup>(2)</sup></span></span> | <span data-ttu-id="db62a-132">7.0</span><span class="sxs-lookup"><span data-stu-id="db62a-132">7.0</span></span>    | <span data-ttu-id="db62a-133">8.0</span><span class="sxs-lookup"><span data-stu-id="db62a-133">8.0</span></span>        | <span data-ttu-id="db62a-134">10.0</span><span class="sxs-lookup"><span data-stu-id="db62a-134">10.0</span></span>            |
| <span data-ttu-id="db62a-135">UWP<sup>(3)</sup></span><span class="sxs-lookup"><span data-stu-id="db62a-135">UWP<sup>(3)</sup></span></span>             | <span data-ttu-id="db62a-136">10.0</span><span class="sxs-lookup"><span data-stu-id="db62a-136">10.0</span></span>   | <span data-ttu-id="db62a-137">10.0.16299</span><span class="sxs-lookup"><span data-stu-id="db62a-137">10.0.16299</span></span> | <span data-ttu-id="db62a-138">TBD</span><span class="sxs-lookup"><span data-stu-id="db62a-138">TBD</span></span>             |
| <span data-ttu-id="db62a-139">Unity<sup>(4)</sup></span><span class="sxs-lookup"><span data-stu-id="db62a-139">Unity<sup>(4)</sup></span></span>           | <span data-ttu-id="db62a-140">2018.1</span><span class="sxs-lookup"><span data-stu-id="db62a-140">2018.1</span></span> | <span data-ttu-id="db62a-141">2018.1</span><span class="sxs-lookup"><span data-stu-id="db62a-141">2018.1</span></span>     | <span data-ttu-id="db62a-142">TBD</span><span class="sxs-lookup"><span data-stu-id="db62a-142">TBD</span></span>             |

<span data-ttu-id="db62a-143"><sup>(1)</sup> 아래 [.NET Framework](#net-framework) 섹션을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="db62a-143"><sup>(1)</sup> See the [.NET Framework](#net-framework) section below.</span></span>

<span data-ttu-id="db62a-144"><sup>(2)</sup> Xamarin에는 EF Core를 사용하여 개발된 일부 애플리케이션이 제대로 작동하지 않을 수 있는 문제 및 알려진 제한이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-144"><sup>(2)</sup> There are issues and known limitations with Xamarin which may prevent some applications developed using EF Core from working correctly.</span></span> <span data-ttu-id="db62a-145">해결 방법은 [활성 문제](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) 목록을 확인하세요.</span><span class="sxs-lookup"><span data-stu-id="db62a-145">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) for workarounds.</span></span>

<span data-ttu-id="db62a-146"><sup>(3)</sup> EF Core 2.0.1 이상을 권장합니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-146"><sup>(3)</sup> EF Core 2.0.1 and newer recommended.</span></span> <span data-ttu-id="db62a-147">[.NET Core UWP 6.x 패키지](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-147">Install the [.NET Core UWP 6.x package](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span></span> <span data-ttu-id="db62a-148">이 문서의 [유니버설 Windows 플랫폼](#universal-windows-platform) 섹션을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="db62a-148">See the [Universal Windows Platform](#universal-windows-platform) section of this article.</span></span>

<span data-ttu-id="db62a-149"><sup>(4)</sup> Unity에는 문제 및 알려진 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-149"><sup>(4)</sup> There are issues and known limitations with Unity.</span></span> <span data-ttu-id="db62a-150">[미해결 문제점](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity) 목록을 확인하세요.</span><span class="sxs-lookup"><span data-stu-id="db62a-150">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span></span>

## <a name="net-framework"></a><span data-ttu-id="db62a-151">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="db62a-151">.NET Framework</span></span>

<span data-ttu-id="db62a-152">.NET Framework를 대상으로 하는 애플리케이션은 .NET Standard 라이브러리와 함께 작동하도록 변경해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-152">Applications that target .NET Framework may need changes to work with .NET Standard libraries:</span></span>

<span data-ttu-id="db62a-153">프로젝트 파일을 편집하고 최초 속성 그룹에 다음 항목이 표시되는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-153">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

<span data-ttu-id="db62a-154">테스트 프로젝트의 경우 다음 항목도 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-154">For test projects, also make sure the following entry is present:</span></span>

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

<span data-ttu-id="db62a-155">이전 버전의 Visual Studio를 사용하려면 [NuGet 클라이언트를 버전 3.6.0으로 업그레이드](https://www.nuget.org/downloads)하여 .NET Standard 2.0 라이브러리와 함께 작동하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-155">If you want to use an older version of Visual Studio, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

<span data-ttu-id="db62a-156">또한 가능한 경우 NuGet packages.config에서 PackageReference로 마이그레이션하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-156">We also recommend migrating from NuGet packages.config to PackageReference if possible.</span></span> <span data-ttu-id="db62a-157">프로젝트 파일에 다음 속성을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-157">Add the following property to your project file:</span></span>

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a><span data-ttu-id="db62a-158">UWP</span><span class="sxs-lookup"><span data-stu-id="db62a-158">Universal Windows Platform</span></span>

<span data-ttu-id="db62a-159">이전 버전의 EF Core 및 .NET UWP에는 여러 가지 호환성 문제가 있었습니다(특히 .NET 네이티브 도구 체인을 사용하여 컴파일된 애플리케이션 관련).</span><span class="sxs-lookup"><span data-stu-id="db62a-159">Earlier versions of EF Core and .NET UWP had numerous compatibility issues, especially with applications compiled with the .NET Native toolchain.</span></span> <span data-ttu-id="db62a-160">새 .NET UWP 버전은 .NET Standard 2.0에 대한 지원을 추가하고 .NET 네이티브 2.0을 포함하여 이전에 보고된 호환성 문제 대부분을 해결했습니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-160">The new .NET UWP version adds support for .NET Standard 2.0 and contains .NET Native 2.0, which fixes most of the compatibility issues previously reported.</span></span> <span data-ttu-id="db62a-161">EF Core 2.0.1은 UWP와 함께 완벽하게 테스트되었지만 테스트가 자동화되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-161">EF Core 2.0.1 has been tested more thoroughly with UWP but testing is not automated.</span></span>

<span data-ttu-id="db62a-162">UWP에서 EF Core를 사용하는 경우:</span><span class="sxs-lookup"><span data-stu-id="db62a-162">When using EF Core on UWP:</span></span>

* <span data-ttu-id="db62a-163">쿼리 성능을 최적화하려면 LINQ 쿼리에서 무명 형식을 사용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-163">To optimize query performance, avoid anonymous types in LINQ queries.</span></span> <span data-ttu-id="db62a-164">UWP 애플리케이션을 앱 스토어에 배포하려면 .NET 네이티브로 애플리케이션을 컴파일해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-164">Deploying a UWP application to the app store requires an application to be compiled with .NET Native.</span></span> <span data-ttu-id="db62a-165">익명 형식의 쿼리는 .NET 네이티브에서 성능이 저하됩니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-165">Queries with anonymous types have worse performance on .NET Native.</span></span>

* <span data-ttu-id="db62a-166">`SaveChanges()` 성능을 최적화하려면 [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy)를 사용하고 엔터티 형식에서 [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx) 및 [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx)를 구현하세요.</span><span class="sxs-lookup"><span data-stu-id="db62a-166">To optimize `SaveChanges()` performance, use [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) and implement [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx), and [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types.</span></span>

## <a name="report-issues"></a><span data-ttu-id="db62a-167">문제 보고</span><span class="sxs-lookup"><span data-stu-id="db62a-167">Report issues</span></span>

<span data-ttu-id="db62a-168">예상대로 작동하지 않는 다른 조합의 경우 [EF Core 문제 추적기](https://github.com/aspnet/entityframeworkcore/issues/new)에 새 문제를 작성해 주시기 바랍니다.</span><span class="sxs-lookup"><span data-stu-id="db62a-168">For any combination that doesn’t work as expected, we encourage creating new issues on the [EF Core issue tracker](https://github.com/aspnet/entityframeworkcore/issues/new).</span></span> <span data-ttu-id="db62a-169">Xamarin 관련 문제는 [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) 또는 [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new)의 문제 추적기를 사용하세요.</span><span class="sxs-lookup"><span data-stu-id="db62a-169">For Xamarin-specific issues use the issue tracker for [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) or [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span></span>
