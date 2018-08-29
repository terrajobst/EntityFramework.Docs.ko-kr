---
title: 모델-EF6 당 여러 다이어그램
author: divega
ms.date: 2016-10-23
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
ms.openlocfilehash: f27afbd3b44ff3eb8044ab960f10b2856a603ee3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993780"
---
# <a name="multiple-diagrams-per-model"></a>모델에 대해 여러 다이어그램
> [!NOTE]
> **EF5 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 5에서 도입 되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

이 비디오 및 페이지에는 Entity Framework Designer (EF 디자이너)를 사용 하 여 여러 다이어그램에 모델을 분할 하는 방법을 보여 줍니다. 모델 보기 또는 편집에 너무 커지면 하는 경우이 기능을 사용 수 있습니다.

이전 버전의 EF 디자이너에서 EDMX 파일 당 한 다이어그램만 있을 수 있습니다. Visual Studio 2012부터 여러 다이어그램에 EDMX 파일을 분할 하는 EF 디자이너를 사용할 수 있습니다.

## <a name="watch-the-video"></a>비디오를 시청 하세요.
이 비디오에는 Entity Framework Designer (EF 디자이너)를 사용 하 여 여러 다이어그램에 모델을 분할 하는 방법을 보여 줍니다. 모델 보기 또는 편집에 너무 커지면 하는 경우이 기능을 사용 수 있습니다.

**제공한**: Julia Kornich

**비디오**: [WMV](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>EF 디자이너 개요

EF 디자이너의 엔터티 데이터 모델 마법사를 사용 하 여 모델을 만들 때.edmx 파일 생성 되어 솔루션에 추가 됩니다. 이 파일에는 엔터티 및 데이터베이스에 매핑하는 방법의 모양을 정의 합니다.

EF 디자이너 다음 구성 요소로 구성 됩니다.

-   모델 편집을 위한 시각적 디자인 화면입니다. 엔터티 및 연결을 만들고, 수정하고, 삭제할 수 있습니다.
-   A **Model 브라우저** 모델의 트리 보기를 제공 하는 창입니다.  엔터티 및 해당 연결 아래에 *\[ModelName\]* 폴더입니다. 데이터베이스 테이블 및 제약 조건 아래에  *\[ModelName\]* 합니다. 폴더에 저장 합니다.
-   A **매핑 정보** 매핑 보기 및 편집에 대 한 창. 엔터티 형식 또는 연결을 데이터베이스 테이블, 열, 저장 프로시저 등에 매핑할 수 있습니다. 

시각적 디자인 화면 창의 엔터티 데이터 모델 마법사가 완료 되 면 자동으로 열립니다. 모델 브라우저 표시 되지 않으면, 기본 디자인 화면을 마우스 오른쪽 단추로 **Model 브라우저**합니다.

다음 스크린샷은 EF 디자이너에서 열린.edmx 파일을 보여 줍니다. 스크린샷은 시각적 디자인 화면 (왼쪽) 및 **Model 브라우저** (오른쪽)에 창입니다.

![EFDesigner2](~/ef6/media/efdesigner2.png)

EF 디자이너에서 수행할 작업을 실행 취소 하려면 Ctrl + Z를 클릭 합니다.

## <a name="working-with-diagrams"></a>다이어그램 작업

기본적으로 EF 디자이너 Diagram1를 호출 하는 한 다이어그램을 만듭니다. 많은 수의 엔터티 및 연결을 사용 하 여 다이어그램에 있는 경우는 가장 원하는 논리적으로 분할 하려고 합니다. Visual Studio 2012부터 여러 다이어그램에서 개념적 모델을 볼 수 있습니다.   

새 다이어그램을 추가 하면 다이어그램 폴더 Model 브라우저 창에 나타납니다. 다이어그램의 이름을 바꾸려면: Model 브라우저 창에서 다이어그램을 선택, 이름 뒤에 한 번 클릭 하 고 새 이름을 입력 합니다.  또한 다이어그램 이름을 마우스 오른쪽 단추로 클릭 하 고 선택할 수 **이름 바꾸기**합니다.

다이어그램 이름은 Visual Studio 편집기에서.edmx 파일 이름 옆에 표시 됩니다. 예를 들어 Model1.edmx\[Diagram1\]합니다.

![DiagramName](~/ef6/media/diagramname.png)

다이어그램 콘텐츠 (모양 및 색의 엔터티 및 연결)에 저장 됩니다는. edmx.diagram 파일입니다. 이 파일을 보려면 솔루션 탐색기를 선택 하 고.edmx 파일을 펼칩니다. 

![DiagramFiles](~/ef6/media/diagramfiles.png)

편집 하지 마십시오는. edmx.diagram EF 디자이너에서 덮어쓸 수 있습니다이 파일의 콘텐츠 파일을 수동으로.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>분할 엔터티 및 새 다이어그램에 연결

기존 다이어그램 (여러 엔터티를 선택 하려면 shift 키를 길게)에서 엔터티를 선택할 수 있습니다. 마우스 오른쪽 단추를 클릭 하 고 선택 **새 다이어그램으로 이동**합니다. 새 다이어그램을 만들고 선택한 엔터티 및 해당 연결 다이어그램으로 이동 됩니다.

Model 브라우저에서 다이어그램 폴더를 마우스 오른쪽 단추로 클릭 하 고 선택할 수 또는 **새 다이어그램을 추가 합니다.** 다음 끌어서 하 고 엔터티를 디자인 화면으로 Model 브라우저에서 엔터티 형식을 폴더 삭제 수 있습니다.

수도 잘라내기 또는 복사 (Ctrl + X 또는 Ctrl + C 키를 사용 하 여) 엔터티 한 다이어그램에서 하 다른 (Ctrl + V 키 사용)을 붙여 넣습니다. 이름이 같은 엔터티는 엔터티를 이미 붙여 넣으려는 다이어그램 있으면 새 엔터티 생성 되어 모델에 추가 됩니다.  예를 들어: Diagram2 부서 엔터티를 포함 합니다. 그런 다음 Diagram2에서 다른 부서를 붙여넣습니다. Department1 엔터티가 생성 되어 개념적 모델에 추가 합니다.   

다이어그램에서 관련된 엔터티를 포함 하려면 rick 원클릭 엔터티 및 선택 **관련 포함**합니다. 이 지정 된 다이어그램에 관련된 엔터티 및 연결의 복사본 하 게 합니다.

## <a name="changing-the-color-of-entities"></a>엔터티는 색 변경

여러 다이어그램에 모델을 분할 하는 것 외에도 엔터티는 색도 변경할 수 있습니다.

색을 변경 하려면 디자인 화면에서 엔터티 (또는 여러 엔터티)를 선택 합니다. 그런 다음 마우스 오른쪽 단추를 클릭 하 고 선택 **속성**합니다. 속성 창에서 선택 합니다 **채우기 색** 속성입니다. 올바른 색 이름 (예를 들어 빨간색) 또는 유효한 RGB (예를 들어, 255, 128, 128)를 사용 하 여 색을 지정 합니다. 

![색](~/ef6/media/color.png)

## <a name="summary"></a>요약

이 항목의 여러 다이어그램에 모델을 분할 하는 방법 및 Entity Framework 디자이너를 사용 하 여 엔터티에 대 한 다른 색을 지정 하는 방법을 살펴보았습니다. 
