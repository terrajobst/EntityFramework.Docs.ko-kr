---
title: Entity Framework 6으로 업그레이드
author: divega
ms.date: 2016-10-23
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 8317fc0dcd5a7b176f3100e449bc6ff708bd310f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997373"
---
# <a name="upgrading-to-entity-framework-6"></a>Entity Framework 6으로 업그레이드

이전 버전의 EF 코드.NET Framework의 일부로 제공 되는 핵심 라이브러리 (주로 System.Data.Entity.dll)와 대역의 (OOB) 라이브러리 (주로 EntityFramework.dll) NuGet 패키지에 제공 간에 분할 되었습니다. EF6 핵심 라이브러리에서 코드 가져와 OOB 라이브러리에 통합 합니다. 이 오픈 소스 되도록 EF를 허용 하는 데 필요한 및.NET Framework에서 서로 다른 속도로 진화 할 수 있도록 합니다. 이 응용 프로그램 이동된 형식에 대해 다시 작성 해야 합니다.

이 EF 4.1 이상 출시 될 때 DbContext를 사용 하는 응용 프로그램에 대 한 간단 하 게 해야 합니다. 좀 더 많은 작업 ObjectContext를 사용 하는 응용 프로그램에 대 한 필요 하지만 것도 어렵지 않습니다.

기존 EF6 응용 프로그램을 업그레이드 하기 위해 필요한 검사는 다음과 같습니다.

## <a name="1-install-the-ef6-nuget-package"></a>1. EF6 NuGet 패키지를 설치 합니다.

새 Entity Framework 6 런타임에 업그레이드 해야 합니다.

1. 선택한 서버 프로젝트를 마우스 오른쪽 단추로 클릭 **NuGet 패키지 관리...**  
2. 아래는 **Online** 탭을 선택 **EntityFramework** 를 클릭 하 고 **설치**  
   > [!NOTE]
   > 이전 버전의 EntityFramework NuGet 패키지를 설치 하는 경우 그러면 EF6에 업그레이드 됩니다.

또는, 패키지 관리자 콘솔에서 다음 명령을 실행할 수 있습니다.

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. System.Data.Entity.dll에 대 한 어셈블리 참조를 제거 해야

EF6 NuGet 패키지를 설치 하면에 대 한 프로젝트에서 System.Data.Entity에 대 한 참조를 자동으로 제거 해야 합니다.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. EF 6.x 코드 생성을 사용 하 여 모든 EF 디자이너 (EDMX) 모델을 교환

EF 디자이너를 사용 하 여 만든 모델에 있는 경우에 EF6 호환 되는 코드를 생성 하도록 코드 생성 템플릿을 업데이트 해야 합니다.

> [!NOTE]
> 현재 가지 EF 6.x DbContext 생성기 템플릿만 Visual Studio 2012 및 2013 사용할 수 있습니다.

1. 기존 코드 생성 템플릿을 삭제 합니다. 이러한 파일 이름은 일반적으로  **\<edmx_file_name\>.tt** 하 고  **\<edmx_file_name\>합니다. Context.tt** 및 솔루션 탐색기에서 edmx 파일을 중첩할 수 있습니다. 키를 눌러 솔루션 탐색기에서 템플릿을 선택할 수 있습니다 합니다 **Del** 삭제할 키입니다.  
   > [!NOTE]
   > 웹 사이트 프로젝트에서 하지 템플릿은 edmx 파일을 아래에 중첩 하 하지만 솔루션 탐색기에서 함께 나열 됩니다.  

   > [!NOTE]
   > VB.NET 프로젝트는 중첩 된 템플릿 파일을 참조 하려면 ' 모든 파일 표시 '를 사용 하도록 설정 해야 합니다.
2. 적절 한 EF 6.x 코드 생성 템플릿을 추가 합니다. 디자인 화면에 열려 EF 디자이너에서 모델 단추로 **코드 생성 항목 추가...**
    - (권장) 한 다음 DbContext API를 사용 하는 경우 **EF 6.x DbContext 생성기** 에서 사용할 수 있습니다 합니다 **데이터** 탭 합니다.  
      > [!NOTE]
      > Visual Studio 2012를 사용 하는 경우이 템플릿이를 EF 6 도구를 설치 해야 합니다. 참조 [Entity Framework 가져오기](~/ef6/fundamentals/install.md) 세부 정보에 대 한 합니다.  

    - ObjectContext API를 사용 하는 경우 선택 해야 합니다 **온라인** 탭 및 검색 **EF 6.x EntityObject 생성기**합니다.  
3. 코드 생성 템플릿을 모든 사용자 지정을 적용 하는 경우 업데이트 된 템플릿을 다시 적용 해야 합니다.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. 사용 중인 모든 core EF 형식의 네임 스페이스를 업데이트 합니다.

DbContext와 Code First 형식에 대 한 네임 스페이스는 변경 되지 않았습니다. 즉, EF 4.1을 사용 하는 대부분의 응용 프로그램 또는 나중에 아무 것도 변경 해야 하지 않습니다.

System.Data.Entity.dll에 이전에 있던 ObjectContext과 같은 형식 새 네임 스페이스로 이동 되었습니다. 즉, 업데이트 해야 하 *를 사용 하 여* 또는 *가져오기* EF6 빌드 지시문입니다.

네임 스페이스 변경 내용에 대 한 일반 규칙 System.Data.*에서 모든 유형의 System.Data.Entity.Core.* 이동 됩니다. 즉, 방금 삽입 **Entity.Core 합니다.** System.Data 후. 예를 들어:

- System.Data.EntityException = > System.Data. **Entity.Core 합니다.** EntityException  
- System.Data.Objects.ObjectContext = > System.Data. **Entity.Core 합니다.** Objects.ObjectContext  
- System.Data.Objects.DataClasses.RelationshipManager = > System.Data. **Entity.Core 합니다.** Objects.DataClasses.RelationshipManager  

이러한 형식에는 *Core* 네임 스페이스는 대부분의 DbContext 기반 응용 프로그램에 대 한 직접 사용 되지 않으므로 합니다. 일부 System.Data.Entity.dll의 일부인 계속 되는 직접 및 일반적으로 DbContext 기반 응용 프로그램에 대 한 형식과 따라서 이동 되지 않은에 *Core* 네임 스페이스입니다. 이러한 항목은 다음과 같습니다.

- System.Data.EntityState = > System.Data. **엔터티.** EntityState  
- System.Data.Objects.DataClasses.EdmFunctionAttribute = > System.Data. **Entity.DbFunctionAttribute**  
  > [!NOTE]
  > 이 클래스의 이름이 바뀌었습니다. 이전 이름 가진 클래스에서 여전히 존재 하 고 작동 있지만 이제 되지 않음으로 표시 합니다.  
- System.Data.Objects.EntityFunctions = > System.Data. **Entity.DbFunctions**  
  > [!NOTE]
  > 이 클래스의 이름이 바뀌었습니다. 이전 이름 가진 클래스 여전히 존재 하 고 작동 있지만 이제 사용 되지 않음으로 표시 합니다.)  
- System.Data.Spatial에서 이동한 공간 클래스 (예: DbGeography, DbGeometry) = > System.Data. **엔터티.** 공간

> [!NOTE]
> System.Data 네임 스페이스의 형식은 EF 어셈블리로 되지 않는 System.Data.dll의 일부입니다. 이러한 형식으로 이동 하지 하 고 있으므로 해당 네임 스페이스가 그대로 유지 됩니다.
