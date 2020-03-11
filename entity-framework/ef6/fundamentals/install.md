---
title: Entity Framework 가져오기-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416098"
---
# <a name="get-entity-framework"></a>Entity Framework 가져오기
Entity Framework는 Visual Studio 및 EF Runtime 용 EF 도구로 구성 됩니다.

## <a name="ef-tools-for-visual-studio"></a>Visual Studio 용 EF 도구

Visual Studio의 Entity Framework Tools에는 EF Designer와 EF Model 마법사가 포함 되어 있으며, 데이터베이스 first 및 Model first 워크플로에 필요 합니다. EF Tools는 모든 최신 버전의 Visual Studio에 포함 되어 있습니다. Visual Studio의 사용자 지정 설치를 수행 하는 경우 해당 항목을 포함 하는 작업을 선택 하거나 개별 구성 요소로 선택 하 여 "Entity Framework 6 도구" 항목을 선택 했는지 확인 해야 합니다.

일부 이전 버전의 Visual Studio에서는 업데이트 된 EF Tools를 다운로드로 사용할 수 있습니다. Visual Studio 버전에서 사용 가능한 최신 버전의 EF Tools를 가져오는 방법에 대 한 지침은 [Visual Studio 버전](~/ef6/what-is-new/visual-studio.md) 을 참조 하세요.

## <a name="ef-runtime"></a>EF 런타임

최신 버전의 Entity Framework는 [Entityframework NuGet 패키지로](https://nuget.org/packages/EntityFramework/)사용할 수 있습니다. NuGet 패키지 관리자에 대해 잘 모르는 경우에는 [Nuget 개요](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow)를 참조 하는 것이 좋습니다.

### <a name="installing-the-ef-nuget-package"></a>EF NuGet 패키지 설치

프로젝트의 **참조** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리 ...** 를 선택 하 여 entityframework 패키지를 설치할 수 있습니다.

![NuGet 패키지 관리](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>패키지 관리자 콘솔에서 설치

또는 [패키지 관리자 콘솔](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)에서 다음 명령을 실행 하 여 entityframework를 설치할 수 있습니다.

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>EF의 특정 버전 설치

EF 4.1부터 새 버전의 EF 런타임이 [Entityframework NuGet 패키지로](https://www.nuget.org/packages/EntityFramework/)릴리스 되었습니다. Visual Studio의 [패키지 관리자 콘솔](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)에서 다음 명령을 실행 하 여 .NET Framework 기반 프로젝트에 이러한 버전을 추가할 수 있습니다.

``` powershell
Install-Package EntityFramework -Version <number>
```

`<number>`은 설치할 EF의 특정 버전을 나타냅니다. 예를 들어 6.2.0는 EF 6.2에 대 한 숫자의 버전입니다.   

4\.1 이전의 EF 런타임은 .NET Framework의 일부 이며 별도로 설치할 수 없습니다.

### <a name="installing-the-latest-preview"></a>최신 미리 보기 설치

위의 메서드는 Entity Framework의 완전히 지원 되는 최신 릴리스를 제공 합니다. 사용자 의견을 보내 주시면 감사를 제공 하는 데 사용할 수 있는 Entity Framework 시험판 버전이 종종 있습니다.

EntityFramework의 최신 미리 보기를 설치 하려면 NuGet 패키지 관리 창에서 **시험판 포함** 을 선택할 수 있습니다. 시험판 버전을 사용할 수 없는 경우 Entity Framework의 완전히 지원 되는 최신 버전을 자동으로 가져옵니다.

![시험판 포함](~/ef6/media/includeprerelease.png)

또는 [패키지 관리자 콘솔](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)에서 다음 명령을 실행할 수 있습니다.

``` powershell
Install-Package EntityFramework -Pre
```
