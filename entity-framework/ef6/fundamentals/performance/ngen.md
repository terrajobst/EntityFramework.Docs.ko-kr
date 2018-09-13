---
title: NGen-EF6 사용 하 여 시작 성능 개선
author: divega
ms.date: 10/23/2016
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: 2e9a93c7741046da25b890fed084a6b280bf5643
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489962"
---
# <a name="improving-startup-performance-with-ngen"></a><span data-ttu-id="0bbba-102">NGen 사용 하 여 시작 성능 개선</span><span class="sxs-lookup"><span data-stu-id="0bbba-102">Improving startup performance with NGen</span></span>
> [!NOTE]
> <span data-ttu-id="0bbba-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="0bbba-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="0bbba-105">.NET Framework는 관리 되는 응용 프로그램에 대 한 네이티브 이미지 생성을 지원 하 고 라이브러리를 통해 응용 프로그램을 빠르게 시작 하는 방법으로 및 일부 경우에도 메모리를 적게 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-105">The .NET Framework supports the generation of native images for managed applications and libraries as a way to help applications start faster and also in some cases use less memory.</span></span> <span data-ttu-id="0bbba-106">네이티브 이미지에서 네이티브 명령을 생성 하지 않아도.NET JIT (Just In Time) 컴파일러 하므로 응용 프로그램을 실행 하기 전에 네이티브 기계어 명령에 포함 된 파일에 관리 코드 어셈블리를 변환 하 여 만들어집니다. 응용 프로그램 런타임입니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-106">Native images are created by translating managed code assemblies into files containing native machine instructions before the application is executed, relieving the .NET JIT (Just-In-Time) compiler from having to generate the native instructions at application runtime.</span></span>  

<span data-ttu-id="0bbba-107">이전 버전 6에서는 EF 런타임의 핵심 라이브러리는.NET Framework의 일부인 및 네이티브 이미지에 자동으로 생성 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-107">Prior to version 6, the EF runtime’s core libraries were part of the .NET Framework and native images were generated automatically for them.</span></span> <span data-ttu-id="0bbba-108">버전 6부터 전체 EF 런타임이 EntityFramework NuGet 패키지로 결합 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-108">Starting with version 6 the whole EF runtime has been combined into the EntityFramework NuGet package.</span></span> <span data-ttu-id="0bbba-109">네이티브 이미지 now 수 생성 NGen.exe 명령줄 도구를 사용 하 여 비슷한 결과 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-109">Native images have to now be generated using the NGen.exe command line tool to obtain similar results.</span></span>  

<span data-ttu-id="0bbba-110">동작이 수행 될 경험적 관찰 EF 런타임 어셈블리의 네이티브 이미지 응용 프로그램 시작 시의 1 ~ 3 초 사이 잘라낼 수 있는지 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-110">Empirical observations show that native images of the EF runtime assemblies can cut between 1 and 3 seconds of application startup time.</span></span>  

## <a name="how-to-use-ngenexe"></a><span data-ttu-id="0bbba-111">NGen.exe를 사용 하는 방법</span><span class="sxs-lookup"><span data-stu-id="0bbba-111">How to use NGen.exe</span></span>  

<span data-ttu-id="0bbba-112">NGen.exe 도구의 가장 기본적인 기능은 "install" 하는 것 (즉,를 만들고 디스크에 유지) 어셈블리 및 모든 해당 직접 종속성에 대 한 네이티브 이미지입니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-112">The most basic function of the NGen.exe tool is to “install” (that is, to create and persist to disk) native images for an assembly and all its direct dependencies.</span></span> <span data-ttu-id="0bbba-113">수행할 수 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-113">Here is how you can achieve that:</span></span>  

1. <span data-ttu-id="0bbba-114">관리자 권한으로 명령 프롬프트 창을 열으십시오</span><span class="sxs-lookup"><span data-stu-id="0bbba-114">Open a Command Prompt window as an administrator</span></span>  
2. <span data-ttu-id="0bbba-115">현재 작업 디렉터리에 대 한 네이티브 이미지를 생성 하려는 어셈블리의 위치로 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-115">Change the current working directory to the location of the assemblies you want to generate native images for:</span></span>  

  ``` console
    cd <*Assemblies location*>  
  ```
3. <span data-ttu-id="0bbba-116">운영 체제 및 응용 프로그램의 구성에 따라 32 비트 아키텍처의 경우 64 비트 아키텍처 또는 둘 다에 대 한 네이티브 이미지를 생성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-116">Depending on your operating system and the application’s configuration you might need to generate native images for 32 bit architecture, 64 bit architecture or for both.</span></span>  

    <span data-ttu-id="0bbba-117">32 비트를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-117">For 32 bit run:</span></span>  
  ``` console
    %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
  ```
    <span data-ttu-id="0bbba-118">64 비트를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-118">For 64 bit run:</span></span>
  ``` console
    %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
  ```

> [!TIP]
> <span data-ttu-id="0bbba-119">매우 일반적인 실수는 잘못 된 아키텍처에 대 한 네이티브 이미지를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-119">Generating native images for the wrong architecture is a very common mistake.</span></span> <span data-ttu-id="0bbba-120">컴퓨터에 설치 된 운영 체제에 적용 되는 모든 아키텍처에 대 한 네이티브 이미지를 간단히 생성할 수 있습니다 하는 확실 하지 않은 경우.</span><span class="sxs-lookup"><span data-stu-id="0bbba-120">When in doubt you can simply generate native images for all the architectures that apply to the operating system installed in the machine.</span></span>  

<span data-ttu-id="0bbba-121">또한 NGen.exe를 제거 하 고 여러 등 공용 이미지의 생성을 큐에 설치 된 네이티브 이미지 표시와 같은 다른 함수를 지원 합니다. 사용량에 대 한 자세한 내용은 읽기를 [NGen.exe 설명서](https://msdn.microsoft.com/library/6t9t5wcf.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-121">NGen.exe also supports other functions such as uninstalling and displaying the installed native images, queuing the generation of multiple images, etc. For more details of usage read the [NGen.exe documentation](https://msdn.microsoft.com/library/6t9t5wcf.aspx).</span></span>  

## <a name="when-to-use-ngenexe"></a><span data-ttu-id="0bbba-122">NGen.exe를 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="0bbba-122">When to use NGen.exe</span></span>  

<span data-ttu-id="0bbba-123">EF 6 이상 버전에 따라 응용 프로그램에서 네이티브 이미지를 생성 하는 어셈블리를 결정할 수 있어 하는 경우에 다음 옵션을 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-123">When it comes to decide which assemblies to generate native images for in an application based on EF version 6 or greater, you should consider the following options:</span></span>  

- <span data-ttu-id="0bbba-124">**주 EF 런타임 어셈블리 EntityFramework.dll**: 일반적인 기반 EF 응용 프로그램 시작 시 또는 첫 번째 액세스에서이 어셈블리에서 데이터베이스에 상당한 양의 코드를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-124">**The main EF runtime assembly, EntityFramework.dll**: A typical EF based application executes a significant amount of code from this assembly on startup or on its first access to the database.</span></span> <span data-ttu-id="0bbba-125">따라서이 어셈블리의 네이티브 이미지 만들기 가장 큰 시작 성능 향상을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-125">Consequently, creating native images of this assembly will produce the biggest gains in startup performance.</span></span>  
- <span data-ttu-id="0bbba-126">**응용 프로그램에서 사용 하는 EF 공급자 어셈블리**: 이러한 네이티브 이미지 생성에서 시작 시간이 약간 활용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-126">**Any EF provider assembly used by your application**: Startup time can also benefit slightly from generating native images of these.</span></span> <span data-ttu-id="0bbba-127">예를 들어 응용 프로그램이 SQL Server에 대 한 EF 공급자를 사용 하는 경우 EntityFramework.SqlServer.dll에 대 한 네이티브 이미지를 생성 하는 것이 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-127">For example, if the application uses the EF provider for SQL Server you will want to generate a native image for EntityFramework.SqlServer.dll.</span></span>  
- <span data-ttu-id="0bbba-128">**응용 프로그램의 어셈블리 및 기타 종속성**: 합니다 [NGen.exe 설명서](https://msdn.microsoft.com/library/6t9t5wcf.aspx) 일반 조건에 대 한 네이티브 이미지 및 네이티브 이미지 보안에 미치는 영향을 생성 하는 어셈블리를 선택 하는 것에 대 한 설명 옵션 "하드 바인딩"와 같은 고급 디버깅 시나리오 등을 프로 파일링에 네이티브 이미지를 사용 하 여 같은 시나리오입니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-128">**Your application’s assemblies and other dependencies**: The [NGen.exe documentation](https://msdn.microsoft.com/library/6t9t5wcf.aspx) covers general criteria for choosing which assemblies to generate native images for and the impact of native images on security, advanced options such as “hard binding”, scenarios such as using native images in debugging and profiling scenarios, etc.</span></span>  

> [!TIP]
> <span data-ttu-id="0bbba-129">신중 하 게 네이티브 이미지를 사용 하 여 시작 성능 및 응용 프로그램의 전반적인 성능에 영향을 측정 하 고 실제 요구 사항을 기준으로 비교 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-129">Make sure you carefully measure the impact of using native images on both the startup performance and the overall performance of your application and compare them against actual requirements.</span></span> <span data-ttu-id="0bbba-130">네이티브 이미지는 일반적으로 시작 성능을 개선 하 고 일부 경우에 메모리 사용량을 낮춥니다. 덕분에, 일부 시나리오를 동일 하 게 활용 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-130">While native images will generally help improve startup up performance and in some cases reduce memory usage, not all scenarios will benefit equally.</span></span> <span data-ttu-id="0bbba-131">예를 들어, 안정적인 상태 실행 (즉, 일단 응용 프로그램에서 사용 중인 모든 메서드가 한 번 이상 호출 된) JIT 컴파일러에서 생성 된 코드 얻을 수 있습니다 사실 네이티브 이미지 보다 성능이 약간 향상.</span><span class="sxs-lookup"><span data-stu-id="0bbba-131">For instance, on steady state execution (that is, once all the methods being used by the application have been invoked at least once) code generated by the JIT compiler can in fact yield slightly better performance than native images.</span></span>  

## <a name="using-ngenexe-in-a-development-machine"></a><span data-ttu-id="0bbba-132">NGen.exe를 사용 하 여 개발 컴퓨터에서</span><span class="sxs-lookup"><span data-stu-id="0bbba-132">Using NGen.exe in a development machine</span></span>  

<span data-ttu-id="0bbba-133">컴파일러는.NET JIT 개발 하는 동안 자주 변경 되는 코드에 대 한 최상의 전체 균형을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-133">During development the .NET JIT compiler will offer the best overall tradeoff for code that is changing frequently.</span></span> <span data-ttu-id="0bbba-134">EF 런타임 어셈블리와 같은 컴파일된 종속성에 대 한 네이티브 이미지 생성 개발 및 테스트 각 실행의 시작 부분에 몇 초를 잘라내기를 가속화 하는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-134">Generating native images for compiled dependencies such as the EF runtime assemblies can help accelerate development and testing by cutting a few seconds out at the beginning of each execution.</span></span>  

<span data-ttu-id="0bbba-135">EF 런타임 어셈블리를 찾는 좋은 출발점이 솔루션에 대 한 NuGet 패키지 위치입니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-135">A good place to find the EF runtime assemblies is the NuGet package location for the solution.</span></span> <span data-ttu-id="0bbba-136">예를 들어 6.0.2 EF를 사용 하 여 SQL server 및.NET 4.5 이상이 대상으로 하는 응용 프로그램에 대해 입력할 수 있습니다 다음 명령 프롬프트 창에서 (관리자로 엽니다 해야 함):</span><span class="sxs-lookup"><span data-stu-id="0bbba-136">For example, for an application using EF 6.0.2 with SQL Server and targeting .NET 4.5 or greater you can type the following in a Command Prompt window (remember to open it as an administrator):</span></span>  

``` console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> <span data-ttu-id="0bbba-137">이 SQL Server에 대 한 EF 공급자에 대 한 네이티브 이미지를 설치 하는 기본적으로 설치 됩니다 네이티브 이미지 주 EF 런타임 어셈블리에 대 한 사실 활용 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-137">This takes advantage of the fact that installing the native images for EF provider for SQL Server will also by default install the native images for the main EF runtime assembly.</span></span> <span data-ttu-id="0bbba-138">이 방법이 NGen.exe EntityFramework.dll 동일한 디렉터리에 있는 EntityFramework.SqlServer.dll 어셈블리의 직접 종속성 임을 감지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-138">This works because NGen.exe can detect that EntityFramework.dll is a direct dependency of the EntityFramework.SqlServer.dll assembly located in the same directory.</span></span>  

## <a name="creating-native-images-during-setup"></a><span data-ttu-id="0bbba-139">설치 하는 동안 네이티브 이미지 만들기</span><span class="sxs-lookup"><span data-stu-id="0bbba-139">Creating native images during setup</span></span>  

<span data-ttu-id="0bbba-140">WiX 도구 키트 설치 하는 동안 관리 되는 어셈블리에 대 한 네이티브 이미지 생성을 큐에 설명 된 대로 지원 [방법 가이드](http://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html)합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-140">The WiX Toolkit supports queuing the generation of native images for managed assemblies during setup, as explained in this [how-to guide](http://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html).</span></span> <span data-ttu-id="0bbba-141">또 다른 방법은 NGen.exe 명령을 실행 하는 사용자 지정 설치 작업을 만드는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-141">Another alternative is to create a custom setup task that execute the NGen.exe command.</span></span>  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a><span data-ttu-id="0bbba-142">EF에 대 한 네이티브 이미지를 사용 중인지를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-142">Verifying that native images are being used for EF</span></span>  

<span data-ttu-id="0bbba-143">특정 응용 프로그램 네이티브 어셈블리를 사용 하 여 확장명을 가진 로드 된 어셈블리를 검색 하 여는 것을 확인할 수 있습니다 ". ni.dll"또는". ni.exe"입니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-143">You can verify that a specific application is using a native assembly by looking for loaded assemblies that have the extension “.ni.dll” or “.ni.exe”.</span></span> <span data-ttu-id="0bbba-144">예를 들어, EF의 기본 런타임 어셈블리에 대 한 네이티브 이미지를 EntityFramework.ni.dll 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-144">For example, a native image for the EF’s main runtime assembly will be called EntityFramework.ni.dll.</span></span> <span data-ttu-id="0bbba-145">프로세스의 로드 된.NET 어셈블리를 검사 하는 쉬운 방법을 사용 하는 것 [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653)합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-145">An easy way to inspect the loaded .NET assemblies of a process is to use [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653).</span></span>  

## <a name="other-things-to-be-aware-of"></a><span data-ttu-id="0bbba-146">고려해 야 할 다른 사항</span><span class="sxs-lookup"><span data-stu-id="0bbba-146">Other things to be aware of</span></span>  

<span data-ttu-id="0bbba-147">**어셈블리의 네이티브 이미지를 만드는 혼동 하지 마십시오에서 어셈블리를 등록 합니다 [GAC (전역 어셈블리 캐시)](https://msdn.microsoft.com/library/yf1d93sz.aspx)** 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-147">**Creating a native image of an assembly should not be confused with registering the assembly in the [GAC (Global Assembly Cache)](https://msdn.microsoft.com/library/yf1d93sz.aspx)**.</span></span> <span data-ttu-id="0bbba-148">NGen.exe GAC에 없는 어셈블리의 이미지를 만들 수 있습니다 하 고 실제로 특정 버전의 EF 사용 하는 여러 응용 프로그램 같은 네이티브 이미지를 공유할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-148">NGen.exe allows creating images of assemblies that are not in the GAC, and in fact, multiple applications that use a specific version of EF can share the same native image.</span></span> <span data-ttu-id="0bbba-149">Windows 8 GAC에 배치 하는 어셈블리에 대 한 네이티브 이미지를 자동으로 만들 수, 하는 동안에 응용 프로그램과 함께 배포할 EF 런타임 최적화 된 및 어셈블리 해상도에 부정적인 영향을 가진이 GAC에 등록 하지 않는 것이 좋습니다 및 다른 측면 중에서 응용 프로그램을 서비스 합니다.</span><span class="sxs-lookup"><span data-stu-id="0bbba-149">While Windows 8 can automatically create native images for assemblies placed in the GAC, the EF runtime is optimized to be deployed alongside your application and we do not recommend registering it in the GAC as this has a negative impact on assembly resolution and servicing your applications among other aspects.</span></span>  
