---
title: NGen을 사용 하 여 시작 성능 향상 EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: c9b5f8a06add9133d30955e3cc97a92e9b189bdf
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182674"
---
# <a name="improving-startup-performance-with-ngen"></a><span data-ttu-id="6242c-102">NGen을 사용 하 여 시작 성능 향상</span><span class="sxs-lookup"><span data-stu-id="6242c-102">Improving startup performance with NGen</span></span>
> [!NOTE]
> <span data-ttu-id="6242c-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="6242c-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="6242c-105">.NET Framework는 관리 되는 응용 프로그램 및 라이브러리에 대 한 네이티브 이미지 생성을 지원 하 여 응용 프로그램을 더 빠르게 시작 하 고 경우에 따라 더 많은 메모리를 사용 하는 방법을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-105">The .NET Framework supports the generation of native images for managed applications and libraries as a way to help applications start faster and also in some cases use less memory.</span></span> <span data-ttu-id="6242c-106">네이티브 이미지는 응용 프로그램이 실행 되기 전에 네이티브 컴퓨터 명령을 포함 하는 파일에 관리 코드 어셈블리를 변환 하 여 생성 됩니다. 하므로 .NET JIT (Just-in-time) 컴파일러는에서 네이티브 명령을 생성 하지 않아도 됩니다. 응용 프로그램 런타임.</span><span class="sxs-lookup"><span data-stu-id="6242c-106">Native images are created by translating managed code assemblies into files containing native machine instructions before the application is executed, relieving the .NET JIT (Just-In-Time) compiler from having to generate the native instructions at application runtime.</span></span>  

<span data-ttu-id="6242c-107">버전 6 이전에서는 EF 런타임의 핵심 라이브러리가 .NET Framework의 일부 이며 네이티브 이미지가 자동으로 생성 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-107">Prior to version 6, the EF runtime’s core libraries were part of the .NET Framework and native images were generated automatically for them.</span></span> <span data-ttu-id="6242c-108">버전 6부터 전체 EF 런타임이 EntityFramework NuGet 패키지로 결합 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-108">Starting with version 6 the whole EF runtime has been combined into the EntityFramework NuGet package.</span></span> <span data-ttu-id="6242c-109">이제 동일한 결과를 얻으려면 Ngen.exe 명령줄 도구를 사용 하 여 네이티브 이미지를 생성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-109">Native images have to now be generated using the NGen.exe command line tool to obtain similar results.</span></span>  

<span data-ttu-id="6242c-110">경험적 관찰은 EF runtime 어셈블리의 네이티브 이미지가 응용 프로그램 시작 시간의 1 ~ 3 초 사이를 잘라낼 수 있음을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-110">Empirical observations show that native images of the EF runtime assemblies can cut between 1 and 3 seconds of application startup time.</span></span>  

## <a name="how-to-use-ngenexe"></a><span data-ttu-id="6242c-111">Ngen.exe를 사용 하는 방법</span><span class="sxs-lookup"><span data-stu-id="6242c-111">How to use NGen.exe</span></span>  

<span data-ttu-id="6242c-112">Ngen.exe 도구의 가장 기본적인 기능은 어셈블리 및 모든 직접 종속성에 대 한 네이티브 이미지를 "설치" (즉, 디스크에 생성 및 유지) 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-112">The most basic function of the NGen.exe tool is to “install” (that is, to create and persist to disk) native images for an assembly and all its direct dependencies.</span></span> <span data-ttu-id="6242c-113">이렇게 하려면 다음을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-113">Here is how you can achieve that:</span></span>  

1. <span data-ttu-id="6242c-114">관리자 권한으로 명령 프롬프트 창을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-114">Open a Command Prompt window as an administrator</span></span>  
2. <span data-ttu-id="6242c-115">현재 작업 디렉터리를 네이티브 이미지를 생성할 어셈블리의 위치로 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-115">Change the current working directory to the location of the assemblies you want to generate native images for:</span></span>  

  ``` console
    cd <*Assemblies location*>  
  ```
3. <span data-ttu-id="6242c-116">운영 체제 및 응용 프로그램의 구성에 따라 32 비트 아키텍처, 64 비트 아키텍처 또는 둘 다에 대 한 네이티브 이미지를 생성 해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-116">Depending on your operating system and the application’s configuration you might need to generate native images for 32 bit architecture, 64 bit architecture or for both.</span></span>  

    <span data-ttu-id="6242c-117">32 비트 실행의 경우:</span><span class="sxs-lookup"><span data-stu-id="6242c-117">For 32 bit run:</span></span>  
  ``` console
    %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
  ```
    <span data-ttu-id="6242c-118">64 비트 실행의 경우:</span><span class="sxs-lookup"><span data-stu-id="6242c-118">For 64 bit run:</span></span>
  ``` console
    %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
  ```

> [!TIP]
> <span data-ttu-id="6242c-119">잘못 된 아키텍처에 대 한 네이티브 이미지를 생성 하는 것은 매우 일반적인 실수입니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-119">Generating native images for the wrong architecture is a very common mistake.</span></span> <span data-ttu-id="6242c-120">확실 하지 않은 경우에는 컴퓨터에 설치 된 운영 체제에 적용 되는 모든 아키텍처에 대해 네이티브 이미지를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-120">When in doubt you can simply generate native images for all the architectures that apply to the operating system installed in the machine.</span></span>  

<span data-ttu-id="6242c-121">또한 Ngen.exe는 설치 된 네이티브 이미지 제거 및 표시, 여러 이미지 생성 대기 등의 다른 함수도 지원 합니다. 사용에 대 한 자세한 내용은 [ngen.exe 설명서](https://msdn.microsoft.com/library/6t9t5wcf.aspx)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="6242c-121">NGen.exe also supports other functions such as uninstalling and displaying the installed native images, queuing the generation of multiple images, etc. For more details of usage read the [NGen.exe documentation](https://msdn.microsoft.com/library/6t9t5wcf.aspx).</span></span>  

## <a name="when-to-use-ngenexe"></a><span data-ttu-id="6242c-122">Ngen.exe를 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="6242c-122">When to use NGen.exe</span></span>  

<span data-ttu-id="6242c-123">EF 버전 6 이상을 기반으로 하는 응용 프로그램에서 네이티브 이미지를 생성할 어셈블리를 결정 하는 경우 다음 옵션을 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-123">When it comes to decide which assemblies to generate native images for in an application based on EF version 6 or greater, you should consider the following options:</span></span>  

- <span data-ttu-id="6242c-124">**주 EF runtime 어셈블리, EntityFramework .dll**: 일반적인 EF 기반 응용 프로그램은 시작 시 또는 데이터베이스에 처음 액세스할 때이 어셈블리에서 상당한 양의 코드를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-124">**The main EF runtime assembly, EntityFramework.dll**: A typical EF based application executes a significant amount of code from this assembly on startup or on its first access to the database.</span></span> <span data-ttu-id="6242c-125">결과적으로이 어셈블리의 네이티브 이미지를 만들면 시작 성능이 가장 크게 향상 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-125">Consequently, creating native images of this assembly will produce the biggest gains in startup performance.</span></span>  
- <span data-ttu-id="6242c-126">**응용 프로그램에서 사용 하는 모든 EF 공급자 어셈블리**: 시작 시간은 이러한 항목의 네이티브 이미지를 생성 하는 데 약간의 이점을 누릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-126">**Any EF provider assembly used by your application**: Startup time can also benefit slightly from generating native images of these.</span></span> <span data-ttu-id="6242c-127">예를 들어 응용 프로그램에서 SQL Server에 대 한 EF 공급자를 사용 하는 경우에는 EntityFramework. SqlServer .dll에 대 한 네이티브 이미지를 생성 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-127">For example, if the application uses the EF provider for SQL Server you will want to generate a native image for EntityFramework.SqlServer.dll.</span></span>  
- <span data-ttu-id="6242c-128">**응용 프로그램의 어셈블리 및 기타 종속성**: [Ngen.exe 설명서](https://msdn.microsoft.com/library/6t9t5wcf.aspx) 는 네이티브 이미지를 생성할 어셈블리를 선택 하 고, 네이티브 이미지를 보안에 적용 하 고, "하드 바인딩"과 같은 고급 옵션, 디버깅에서 네이티브 이미지 사용과 같은 시나리오에 대 한 일반적인 조건을 설명 합니다. 프로 파일링 시나리오 등</span><span class="sxs-lookup"><span data-stu-id="6242c-128">**Your application’s assemblies and other dependencies**: The [NGen.exe documentation](https://msdn.microsoft.com/library/6t9t5wcf.aspx) covers general criteria for choosing which assemblies to generate native images for and the impact of native images on security, advanced options such as “hard binding”, scenarios such as using native images in debugging and profiling scenarios, etc.</span></span>  

> [!TIP]
> <span data-ttu-id="6242c-129">응용 프로그램의 시작 성능 및 전반적인 성능 모두에서 네이티브 이미지를 사용 하는 경우의 영향을 신중 하 게 측정 하 여 실제 요구 사항과 비교 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-129">Make sure you carefully measure the impact of using native images on both the startup performance and the overall performance of your application and compare them against actual requirements.</span></span> <span data-ttu-id="6242c-130">일반적으로 네이티브 이미지는 시작 성능을 개선 하는 데 도움이 되지만 일부 경우에는 메모리 사용량을 줄일 수 있지만 일부 시나리오에서는 동일한 이점을 누릴 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-130">While native images will generally help improve startup up performance and in some cases reduce memory usage, not all scenarios will benefit equally.</span></span> <span data-ttu-id="6242c-131">예를 들어, 안정적인 상태 실행에서 (즉, 응용 프로그램에서 사용 하는 모든 메서드가 한 번 이상 호출 된 경우) JIT 컴파일러에서 생성 된 코드는 실제로 네이티브 이미지 보다 성능이 약간 향상 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-131">For instance, on steady state execution (that is, once all the methods being used by the application have been invoked at least once) code generated by the JIT compiler can in fact yield slightly better performance than native images.</span></span>  

## <a name="using-ngenexe-in-a-development-machine"></a><span data-ttu-id="6242c-132">개발 컴퓨터에서 Ngen.exe 사용</span><span class="sxs-lookup"><span data-stu-id="6242c-132">Using NGen.exe in a development machine</span></span>  

<span data-ttu-id="6242c-133">개발 하는 동안 .NET JIT 컴파일러는 자주 변경 되는 코드에 대 한 최상의 전반적인 균형을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-133">During development the .NET JIT compiler will offer the best overall tradeoff for code that is changing frequently.</span></span> <span data-ttu-id="6242c-134">EF runtime 어셈블리와 같은 컴파일된 종속성에 대 한 네이티브 이미지를 생성 하면 각 실행이 시작 될 때 몇 초 정도 축소 하 여 개발과 테스트 속도를 높일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-134">Generating native images for compiled dependencies such as the EF runtime assemblies can help accelerate development and testing by cutting a few seconds out at the beginning of each execution.</span></span>  

<span data-ttu-id="6242c-135">EF runtime 어셈블리를 찾는 좋은 위치는 솔루션에 대 한 NuGet 패키지 위치입니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-135">A good place to find the EF runtime assemblies is the NuGet package location for the solution.</span></span> <span data-ttu-id="6242c-136">예를 들어 SQL Server에서 EF 6.0.2를 사용 하 고 .NET 4.5 이상을 대상으로 하는 응용 프로그램의 경우 명령 프롬프트 창에 다음을 입력할 수 있습니다 (관리자로 열어야 합니다).</span><span class="sxs-lookup"><span data-stu-id="6242c-136">For example, for an application using EF 6.0.2 with SQL Server and targeting .NET 4.5 or greater you can type the following in a Command Prompt window (remember to open it as an administrator):</span></span>  

```console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> <span data-ttu-id="6242c-137">이를 통해 EF provider for SQL Server에 대 한 네이티브 이미지를 설치 하는 것은 기본적으로 주 EF runtime 어셈블리에 대 한 네이티브 이미지를 설치 한다는 이점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-137">This takes advantage of the fact that installing the native images for EF provider for SQL Server will also by default install the native images for the main EF runtime assembly.</span></span> <span data-ttu-id="6242c-138">Ngen.exe는 EntityFramework .dll이 동일한 디렉터리에 있는 EntityFramework. SqlServer .dll 어셈블리와 직접 종속 되어 있음을 감지할 수 있기 때문에이 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-138">This works because NGen.exe can detect that EntityFramework.dll is a direct dependency of the EntityFramework.SqlServer.dll assembly located in the same directory.</span></span>  

## <a name="creating-native-images-during-setup"></a><span data-ttu-id="6242c-139">설치 하는 동안 네이티브 이미지 만들기</span><span class="sxs-lookup"><span data-stu-id="6242c-139">Creating native images during setup</span></span>  

<span data-ttu-id="6242c-140">WiX 도구 키트는이 [방법 가이드](https://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html)에 설명 된 대로 설치 하는 동안 관리 되는 어셈블리의 네이티브 이미지 생성을 큐에 대기 하도록 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-140">The WiX Toolkit supports queuing the generation of native images for managed assemblies during setup, as explained in this [how-to guide](https://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html).</span></span> <span data-ttu-id="6242c-141">또 다른 방법은 Ngen.exe 명령을 실행 하는 사용자 지정 설치 작업을 만드는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-141">Another alternative is to create a custom setup task that execute the NGen.exe command.</span></span>  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a><span data-ttu-id="6242c-142">네이티브 이미지가 EF에 사용 되 고 있는지 확인 하는 중</span><span class="sxs-lookup"><span data-stu-id="6242c-142">Verifying that native images are being used for EF</span></span>  

<span data-ttu-id="6242c-143">확장명이 ".ni" 또는 "...exe" 인 로드 된 어셈블리를 검색 하 여 특정 응용 프로그램이 네이티브 어셈블리를 사용 하 고 있는지 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-143">You can verify that a specific application is using a native assembly by looking for loaded assemblies that have the extension “.ni.dll” or “.ni.exe”.</span></span> <span data-ttu-id="6242c-144">예를 들어 EF의 주 런타임 어셈블리에 대 한 네이티브 이미지를 EntityFramework. t s d .dll 이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-144">For example, a native image for the EF’s main runtime assembly will be called EntityFramework.ni.dll.</span></span> <span data-ttu-id="6242c-145">프로세스의 로드 된 .NET 어셈블리를 검사 하는 쉬운 방법은 [프로세스 탐색기](https://technet.microsoft.com/sysinternals/bb896653)를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-145">An easy way to inspect the loaded .NET assemblies of a process is to use [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653).</span></span>  

## <a name="other-things-to-be-aware-of"></a><span data-ttu-id="6242c-146">유의 해야 할 기타 사항</span><span class="sxs-lookup"><span data-stu-id="6242c-146">Other things to be aware of</span></span>  

<span data-ttu-id="6242c-147">**어셈블리의 네이티브 이미지를 만들려면 [GAC (전역 어셈블리 캐시)](https://msdn.microsoft.com/library/yf1d93sz.aspx)에 어셈블리를 등록 하는 것과 혼동**해서는 안 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-147">**Creating a native image of an assembly should not be confused with registering the assembly in the [GAC (Global Assembly Cache)](https://msdn.microsoft.com/library/yf1d93sz.aspx)**.</span></span> <span data-ttu-id="6242c-148">Ngen.exe를 사용 하면 GAC에 없는 어셈블리의 이미지를 만들 수 있으며, 실제로 특정 버전의 EF를 사용 하는 여러 응용 프로그램은 동일한 네이티브 이미지를 공유할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-148">NGen.exe allows creating images of assemblies that are not in the GAC, and in fact, multiple applications that use a specific version of EF can share the same native image.</span></span> <span data-ttu-id="6242c-149">Windows 8은 GAC에 배치 된 어셈블리에 대 한 네이티브 이미지를 자동으로 만들 수 있지만, EF 런타임은 응용 프로그램과 함께 배포 되도록 최적화 되며, 어셈블리 확인에 부정적인 영향을 주므로 GAC에 등록 하는 것이 좋습니다. 다른 측면에서 응용 프로그램을 서비스 합니다.</span><span class="sxs-lookup"><span data-stu-id="6242c-149">While Windows 8 can automatically create native images for assemblies placed in the GAC, the EF runtime is optimized to be deployed alongside your application and we do not recommend registering it in the GAC as this has a negative impact on assembly resolution and servicing your applications among other aspects.</span></span>  
