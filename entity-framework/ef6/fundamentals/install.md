---
title: Entity Framework-EF6 가져오기
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
caps.latest.revision: 4
ms.openlocfilehash: 400bf1428e6754a88dbc1264c346bb66282725a0
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122753"
---
# <a name="get-entity-framework"></a>Entity Framework 가져오기
Entity Framework는 Visual Studio 및 EF 런타임에 대 한 EF 도구 구성 됩니다.

## <a name="ef-tools-for-visual-studio"></a>Visual Studio 용 EF 도구

Entity Framework Tools for Visual Studio는 먼저 데이터베이스에 필요한 고 첫 번째 워크플로 모델 EF 디자이너 및 EF 모델 마법사를 포함 합니다. EF 도구는 모든 최신 버전의 Visual Studio에 포함 됩니다. 항목이 있는지 확인 해야 하는 Visual Studio의 사용자 지정 설치를 수행 하는 경우 "Entity Framework 6 도구" 하거나 포함 하는 작업을 선택 하 여 선택 하거나 개별 구성 요소로 선택 됩니다.

일부 이전 버전의 Visual Studio에 대 한 업데이트 된 EF 도구는 다운로드로 제공 됩니다. 참조 [Visual Studio 버전](~/ef6/what-is-new/visual-studio.md) Visual Studio의 버전에 대 한 EF 도구를 사용할 수 있는 최신 버전을 가져오는 방법에 대 한 지침에 대 한 합니다.

## <a name="ef-runtime"></a>EF 런타임

Entity Framework의 최신 버전은 제공 된 [EntityFramework NuGet 패키지](http://nuget.org/packages/EntityFramework/)합니다. NuGet 패키지 관리자를 사용 하 여 잘 모르는 경우 의견을 교환하실 수 읽기를 [NuGet 개요](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow)합니다.

### <a name="installing-the-ef-nuget-package"></a>EF NuGet 패키지를 설치합니다.

마우스 오른쪽 단추로 클릭 하 여 EntityFramework 패키지를 설치할 수 있습니다 합니다 **참조가** 프로젝트의 폴더를 선택 하 고 **NuGet 패키지 관리...**

![ManageNuGetPackages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>패키지 관리자 콘솔에서 설치

EntityFramework에서 다음 명령을 실행 하 여 설치할 수 있습니다 또는 합니다 [패키지 관리자 콘솔](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)합니다.

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>EF의 특정 버전 설치

EF 4.1 이상에서 새 버전의 EF 런타임이 보기로 출시 되었으며 합니다 [EntityFramework NuGet 패키지](https://www.nuget.org/packages/EntityFramework/)합니다. Visual Studio에서 다음 명령을 실행 하 여.NET Framework 기반 프로젝트에 추가할 수 있습니다 이러한 버전 중 하나 [패키지 관리자 콘솔](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

``` powershell
Install-Package EntityFramework -Version <number>
```

`<number>` 를 설치 하는 EF의 특정 버전을 나타냅니다. 예를 들어 6.2.0 EF 6.2에 대 한 수의 버전입니다.   

EF 런타임 전에 4.1.NET Framework의 일부인 것을 별도로 설치할 수 없습니다.

### <a name="installing-the-latest-preview"></a>최신 미리 보기 설치

위의 방법이 표시 됩니다 최신 릴리스의 Entity Framework를 완벽 하 게 지원 합니다. 시험판 버전의 Entity Framework는 환영 사용해에 대 한 의견을 사용할 수 있는 많습니다.

EntityFramework 선택할 수 있습니다의 최신 미리 보기를 설치 하려면 **시험판 포함** NuGet 패키지 관리 창에서. 자동으로 최신 시험판 버전을 사용할 수 있는 경우 받습니다 완벽 하 게 지원 되는 Entity Framework의 버전입니다.

![IncludePreRelease](~/ef6/media/includeprerelease.png)

다음 명령을 실행할 수는 또는 [패키지 관리자 콘솔](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)합니다.

``` powershell
Install-Package EntityFramework -Pre
```
