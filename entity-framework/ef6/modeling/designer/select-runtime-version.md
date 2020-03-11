---
title: EF Designer 모델에 대 한 Entity Framework 런타임 버전 선택-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414994"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>EF 디자이너 모델에 대 한 Entity Framework 런타임 버전 선택
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

EF6부터 다음 화면이 EF 디자이너에 추가 되어 모델을 만들 때 대상으로 지정할 런타임의 버전을 선택할 수 있습니다. 최신 버전의 Entity Framework 프로젝트에 아직 설치 되지 않은 경우 화면이 표시 됩니다. 최신 버전이 이미 설치 되어 있는 경우에는 기본적으로 사용 됩니다.

![화면](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>EF6를 대상으로 지정

' 버전 선택 ' 화면에서 EF6를 선택 하 여 프로젝트에 EF6 런타임을 추가할 수 있습니다. EF6를 추가 하면 현재 프로젝트에서이 화면이 표시 되지 않습니다.

이전 버전의 EF가 이미 설치 되어 있는 경우에는 EF6가 사용 하지 않도록 설정 됩니다. 동일한 프로젝트에서 여러 버전의 런타임을 대상으로 지정할 수 없기 때문입니다. EF6 옵션을 사용 하도록 설정 하지 않은 경우 다음 단계에 따라 프로젝트를 EF6로 업그레이드 합니다.

1.  솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리 ...** 를 선택 합니다.
2.  **업데이트** 선택
3.  **Entityframework** (원하는 버전으로 업데이트 해야 하는지 확인)를 선택 합니다.
4.  **업데이트**를 클릭합니다.

 

## <a name="targeting-ef5x"></a>EF5를 대상으로 지정

' 버전 선택 ' 화면에서 EF5를 선택 하 여 프로젝트에 EF5 런타임을 추가할 수 있습니다. EF5를 추가 하면 EF6 옵션이 비활성화 된 상태로 화면이 계속 표시 됩니다.

EF4 버전이 이미 설치 되어 있는 경우에는 EF5 대신 EF 버전이 화면에 표시 됩니다. 이 경우 다음 단계를 사용 하 여 EF5로 업그레이드할 수 있습니다.

1.  **도구-&gt; 라이브러리 패키지 관리자-&gt; 패키지 관리자 콘솔을** 선택 합니다.
2.  **설치-패키지 EntityFramework-버전 5.0.0을** 실행 합니다.

 

## <a name="targeting-ef4x"></a>EF4를 대상으로 지정

다음 단계를 사용 하 여 EF4 런타임을 프로젝트에 설치할 수 있습니다.

1.  **도구-&gt; 라이브러리 패키지 관리자-&gt; 패키지 관리자 콘솔을** 선택 합니다.
2.  **설치-패키지 EntityFramework-버전 4.3.0을** 실행 합니다.
