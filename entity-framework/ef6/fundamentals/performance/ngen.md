---
title: NGen을 사용 하 여 시작 성능 향상 EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: 841aec645abdb2a56076d0b70bfb2614b0acafb4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416092"
---
# <a name="improving-startup-performance-with-ngen"></a>NGen을 사용 하 여 시작 성능 향상
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

.NET Framework는 관리 되는 응용 프로그램 및 라이브러리에 대 한 네이티브 이미지 생성을 지원 하 여 응용 프로그램을 더 빠르게 시작 하 고 경우에 따라 더 많은 메모리를 사용 하는 방법을 지원 합니다. 네이티브 이미지는 응용 프로그램이 실행 되기 전에 네이티브 컴퓨터 명령을 포함 하는 파일에 관리 코드 어셈블리를 변환 하 여 생성 됩니다. 하므로 .NET JIT (Just-in-time) 컴파일러는에서 네이티브 명령을 생성 하지 않아도 됩니다. 응용 프로그램 런타임.  

버전 6 이전에서는 EF 런타임의 핵심 라이브러리가 .NET Framework의 일부 이며 네이티브 이미지가 자동으로 생성 되었습니다. 버전 6부터 전체 EF 런타임이 EntityFramework NuGet 패키지로 결합 되었습니다. 이제 동일한 결과를 얻으려면 Ngen.exe 명령줄 도구를 사용 하 여 네이티브 이미지를 생성 해야 합니다.  

경험적 관찰은 EF runtime 어셈블리의 네이티브 이미지가 응용 프로그램 시작 시간의 1 ~ 3 초 사이를 잘라낼 수 있음을 보여 줍니다.  

## <a name="how-to-use-ngenexe"></a>Ngen.exe를 사용 하는 방법  

Ngen.exe 도구의 가장 기본적인 기능은 어셈블리 및 모든 직접 종속성에 대 한 네이티브 이미지를 "설치" (즉, 디스크에 생성 및 유지) 하는 것입니다. 이렇게 하려면 다음을 수행 합니다.  

1. 관리자로 명령 프롬프트 창을 엽니다.
2. 현재 작업 디렉터리를 네이티브 이미지를 생성할 어셈블리의 위치로 변경 합니다.

   ``` console
   cd <*Assemblies location*>  
   ```

3. 운영 체제 및 응용 프로그램의 구성에 따라 32 비트 아키텍처, 64 비트 아키텍처 또는 둘 다에 대 한 네이티브 이미지를 생성 해야 할 수 있습니다.

   32 비트 실행의 경우:

   ``` console
   %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
   ```

   64 비트 실행의 경우:
  
   ``` console
   %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
   ```

> [!TIP]
> 잘못 된 아키텍처에 대 한 네이티브 이미지를 생성 하는 것은 매우 일반적인 실수입니다. 확실 하지 않은 경우에는 컴퓨터에 설치 된 운영 체제에 적용 되는 모든 아키텍처에 대해 네이티브 이미지를 생성할 수 있습니다.  

또한 Ngen.exe는 설치 된 네이티브 이미지 제거 및 표시, 여러 이미지 생성 대기 등의 다른 함수도 지원 합니다. 사용에 대 한 자세한 내용은 [ngen.exe 설명서](https://msdn.microsoft.com/library/6t9t5wcf.aspx)를 참조 하세요.  

## <a name="when-to-use-ngenexe"></a>Ngen.exe를 사용 하는 경우  

EF 버전 6 이상을 기반으로 하는 응용 프로그램에서 네이티브 이미지를 생성할 어셈블리를 결정 하는 경우 다음 옵션을 고려해 야 합니다.  

- **주 ef runtime 어셈블리인 EntityFramework .dll**: 일반적인 ef 기반 응용 프로그램은 시작 시 또는 데이터베이스에 처음 액세스할 때이 어셈블리에서 상당한 양의 코드를 실행 합니다. 결과적으로이 어셈블리의 네이티브 이미지를 만들면 시작 성능이 가장 크게 향상 됩니다.  
- **응용 프로그램에서 사용 하는 모든 EF 공급자 어셈블리**: 시작 시간은 이러한 응용 프로그램의 네이티브 이미지를 생성할 때 약간의 이점을 누릴 수 있습니다. 예를 들어 응용 프로그램에서 SQL Server에 대 한 EF 공급자를 사용 하는 경우에는 EntityFramework. SqlServer .dll에 대 한 네이티브 이미지를 생성 하는 것이 좋습니다.  
- **응용 프로그램의 어셈블리 및 기타 종속성**: [Ngen.exe 설명서](https://msdn.microsoft.com/library/6t9t5wcf.aspx) 에는 네이티브 이미지를 생성할 어셈블리를 선택 하 고, 네이티브 이미지를 보안에 적용 하 고, "하드 바인딩"과 같은 고급 옵션, 디버깅 및 프로 파일링 시나리오에서 네이티브 이미지를 사용 하는 경우와 같은 시나리오 등의 일반적인 기준이 포함 되어 있습니다.  

> [!TIP]
> 응용 프로그램의 시작 성능 및 전반적인 성능 모두에서 네이티브 이미지를 사용 하는 경우의 영향을 신중 하 게 측정 하 여 실제 요구 사항과 비교 해야 합니다. 일반적으로 네이티브 이미지는 시작 성능을 개선 하는 데 도움이 되지만 일부 경우에는 메모리 사용량을 줄일 수 있지만 일부 시나리오에서는 동일한 이점을 누릴 수 없습니다. 예를 들어, 안정적인 상태 실행에서 (즉, 응용 프로그램에서 사용 하는 모든 메서드가 한 번 이상 호출 된 경우) JIT 컴파일러에서 생성 된 코드는 실제로 네이티브 이미지 보다 성능이 약간 향상 될 수 있습니다.  

## <a name="using-ngenexe-in-a-development-machine"></a>개발 컴퓨터에서 Ngen.exe 사용  

개발 하는 동안 .NET JIT 컴파일러는 자주 변경 되는 코드에 대 한 최상의 전반적인 균형을 제공 합니다. EF runtime 어셈블리와 같은 컴파일된 종속성에 대 한 네이티브 이미지를 생성 하면 각 실행이 시작 될 때 몇 초 정도 축소 하 여 개발과 테스트 속도를 높일 수 있습니다.  

EF runtime 어셈블리를 찾는 좋은 위치는 솔루션에 대 한 NuGet 패키지 위치입니다. 예를 들어 SQL Server에서 EF 6.0.2를 사용 하 고 .NET 4.5 이상을 대상으로 하는 응용 프로그램의 경우 명령 프롬프트 창에 다음을 입력할 수 있습니다 (관리자로 열어야 합니다).  

```console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> 이를 통해 EF provider for SQL Server에 대 한 네이티브 이미지를 설치 하는 것은 기본적으로 주 EF runtime 어셈블리에 대 한 네이티브 이미지를 설치 한다는 이점이 있습니다. Ngen.exe는 EntityFramework .dll이 동일한 디렉터리에 있는 EntityFramework. SqlServer .dll 어셈블리와 직접 종속 되어 있음을 감지할 수 있기 때문에이 작업을 수행할 수 있습니다.  

## <a name="creating-native-images-during-setup"></a>설치 하는 동안 네이티브 이미지 만들기  

WiX 도구 키트는이 [방법 가이드](https://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html)에 설명 된 대로 설치 하는 동안 관리 되는 어셈블리의 네이티브 이미지 생성을 큐에 대기 하도록 지원 합니다. 또 다른 방법은 Ngen.exe 명령을 실행 하는 사용자 지정 설치 작업을 만드는 것입니다.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>네이티브 이미지가 EF에 사용 되 고 있는지 확인 하는 중  

확장명이 ".ni" 또는 "...exe" 인 로드 된 어셈블리를 검색 하 여 특정 응용 프로그램이 네이티브 어셈블리를 사용 하 고 있는지 확인할 수 있습니다. 예를 들어 EF의 주 런타임 어셈블리에 대 한 네이티브 이미지를 EntityFramework. t s d .dll 이라고 합니다. 프로세스의 로드 된 .NET 어셈블리를 검사 하는 쉬운 방법은 [프로세스 탐색기](https://technet.microsoft.com/sysinternals/bb896653)를 사용 하는 것입니다.  

## <a name="other-things-to-be-aware-of"></a>유의 해야 할 기타 사항  

**어셈블리의 네이티브 이미지를 만들려면 [GAC (전역 어셈블리 캐시)](https://msdn.microsoft.com/library/yf1d93sz.aspx)에 어셈블리를 등록 하는 것과 혼동**해서는 안 됩니다. Ngen.exe를 사용 하면 GAC에 없는 어셈블리의 이미지를 만들 수 있으며, 실제로 특정 버전의 EF를 사용 하는 여러 응용 프로그램은 동일한 네이티브 이미지를 공유할 수 있습니다. Windows 8은 GAC에 배치 된 어셈블리에 대 한 네이티브 이미지를 자동으로 만들 수 있지만, EF 런타임은 응용 프로그램과 함께 배포 되도록 최적화 되며, 어셈블리 확인에 부정적인 영향을 주므로 GAC에 등록 하는 것이 좋습니다. 다른 측면에서 응용 프로그램을 서비스 합니다.  
