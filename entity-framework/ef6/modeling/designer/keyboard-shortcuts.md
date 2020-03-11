---
title: Entity Framework Designer 키보드 바로 가기-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
ms.openlocfilehash: c75eafcca0863faa1ad64202e98b61832827377c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415312"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Entity Framework Designer 바로 가기 키
이 페이지에서는 Visual Studio Entity Framework Tools의 다양 한 화면에서 사용할 수 있는 키보드 shorcuts 목록을 제공 합니다.

## <a name="adonet-entity-data-model-wizard"></a>ADO.NET 엔터티 데이터 모델 마법사

### <a name="step-one-choose-model-contents"></a>1 단계: 모델 콘텐츠 선택

![마법사 One](~/ef6/media/wizardone.png)

| 바로 가기  | 작업                                                     | 참고                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **Alt + n** | 다음 화면으로 이동                                        | 모델 콘텐츠의 일부 선택에는 사용할 수 없습니다. |
| **Alt + f** | 마법사를 완료하십시오.                                              | 모델 콘텐츠의 일부 선택에는 사용할 수 없습니다. |
| **Alt + w** | "모델에 포함 되어야 하는 내용"으로 포커스를 전환 합니다. 창에 WCF 서비스가 나열됩니다. |                                                     |

### <a name="step-two-choose-your-connection"></a>2 단계: 연결 선택

![마법사 2](~/ef6/media/wizardtwo.png)

| 바로 가기  | 작업                                                     | 참고                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **Alt + n** | 다음 화면으로 이동                                        |                                                         |
| **Alt + p** | 이전 화면으로 이동                                    |                                                         |
| **Alt + w** | "모델에 포함 되어야 하는 내용"으로 포커스를 전환 합니다. 창에 WCF 서비스가 나열됩니다. |                                                         |
| **Alt + c** | "연결 속성" 창을 엽니다.                    | 새 데이터베이스 연결을 정의할 수 있습니다. |
| **Alt + e** | 연결 문자열에서 중요 한 데이터를 제외 합니다.          |                                                         |
| **Alt + i** | 연결 문자열에 중요 한 데이터 포함            |                                                         |
| **Alt + s** | "App.config의 연결 설정 저장" 옵션을 설정/해제 합니다. |                                                         |

### <a name="step-three-choose-your-version"></a>3 단계: 버전 선택

![마법사 3](~/ef6/media/wizardthree.png)

| 바로 가기  | 작업                                             | 참고                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **Alt + n** | 다음 화면으로 이동                                |                                                                                       |
| **Alt + p** | 이전 화면으로 이동                            |                                                                                       |
| **Alt + w** | 포커스를 Entity Framework 버전 선택으로 전환 | 프로젝트에 사용할 다른 버전의 Entity Framework을 지정할 수 있습니다. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>4 단계: 데이터베이스 개체 및 설정 선택

![마법사 4](~/ef6/media/wizardfour.png)

| 바로 가기  | 작업                                                                                    | 참고                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **Alt + f** | 마법사를 완료하십시오.                                                                             |                                                                     |
| **Alt + p** | 이전 화면으로 이동                                                                   |                                                                     |
| **Alt + w** | 데이터베이스 개체 선택 창으로 포커스 전환                                            | 데이터베이스 개체를 리버스 엔지니어링할 수 있도록 지정할 수 있습니다.    |
| **Alt + s** | "생성 된 개체 이름 복수화 또는 단 수" 옵션을 설정/해제 합니다.                       |                                                                     |
| **Alt + k** | "모델에 외래 키 열 포함" 옵션을 설정/해제 합니다.                              | 모델 콘텐츠의 일부 선택에는 사용할 수 없습니다.                 |
| **Alt + i** | "선택한 저장 프로시저 및 함수를 엔터티 모델로 가져오기" 옵션을 설정/해제 합니다. | 모델 콘텐츠의 일부 선택에는 사용할 수 없습니다.                 |
| **Alt + m** | "모델 네임 스페이스" 텍스트 필드로 포커스를 전환 합니다.                                        | 모델 콘텐츠의 일부 선택에는 사용할 수 없습니다.                 |
| **스페이스바** | 요소에 대 한 선택 설정/해제                                                               | 요소에 자식이 있으면 모든 자식 요소도 설정/해제 됩니다. |
| **왼쪽**  | 자식 트리 축소                                                                       |                                                                     |
| **오른쪽** | 자식 트리 확장                                                                         |                                                                     |
| **위로**    | 트리의 이전 요소로 이동                                                      |                                                                     |
| **아래로**  | 트리의 다음 요소로 이동 합니다.                                                          |                                                                     |

## <a name="ef-designer-surface"></a>EF 디자이너 화면

![디자이너 화면](~/ef6/media/designersurface.png)

| 바로 가기                                                                                | 작업                      | 참고                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **공백/입력**                                                                         | 선택 영역 전환            | 포커스를 사용 하 여 개체에서 선택을 전환 합니다.                                                                                                                                                                                         |
| **Esc**                                                                                 | 선택 취소            | 현재 선택을 취소합니다.                                                                                                                                                                                                      |
| **Ctrl + A**                                                                            | 모두 선택                  | 디자인 화면의 모든 셰이프를 선택 합니다.                                                                                                                                                                                       |
| **위쪽 화살표**                                                                            | 위로 이동                     | 선택한 엔터티를 하나의 그리드 증분으로 이동 합니다. <br/> 목록에 있는 경우 이전 형제 하위 필드를 이동 합니다.                                                                                                                            |
| **아래쪽 화살표**                                                                          | 아래로 이동                   | 선택한 엔터티를 하나의 그리드 증분으로 이동 합니다. <br/> 목록에 있는 경우 다음 형제 하위 필드를 이동 합니다.                                                                                                                              |
| **왼쪽 화살표**                                                                          | 왼쪽으로 이동                   | 선택한 엔터티를 한 모눈 씩 왼쪽으로 이동 합니다. <br/> 목록에 있는 경우 이전 형제 하위 필드를 이동 합니다.                                                                                                                          |
| **오른쪽 화살표**                                                                         | 오른쪽으로 이동                  | 선택한 엔터티를 한 모눈 씩 오른쪽으로 이동 합니다. <br/> 목록에 있는 경우 다음 형제 하위 필드를 이동 합니다.                                                                                                                             |
| **Shift + 왼쪽 화살표**                                                                  | 왼쪽으로 셰이프 크기 조정             | 선택한 엔터티의 너비를 한 모눈 씩 줄입니다.                                                                                                                                                                     |
| **Shift + 오른쪽 화살표**                                                                 | 오른쪽으로 셰이프 크기 조정            | 선택한 엔터티의 너비를 하나의 그리드 증분으로 늘립니다.                                                                                                                                                                   |
| **Home**                                                                                | 첫 번째 피어                  | 포커스 및 선택 영역을 디자인 화면에서 동일한 피어 수준에 있는 첫 번째 개체로 이동 합니다.                                                                                                                                         |
| **End**                                                                                 | 마지막 피어                   | 포커스 및 선택 영역을 디자인 화면에서 동일한 피어 수준에 있는 마지막 개체로 이동 합니다.                                                                                                                                          |
| **Ctrl + Home**                                                                         | 첫 번째 피어 (포커스)          | 첫 번째 피어와 동일 하지만 포커스 및 선택 영역을 이동 하는 대신 포커스를 이동 합니다.                                                                                                                                                          |
| **Ctrl + End**                                                                          | 마지막 피어 (포커스)           | 마지막 피어와 동일 하지만 포커스 및 선택 영역을 이동 하는 대신 포커스를 이동 합니다.                                                                                                                                                           |
| **Tab**                                                                                 | 다음 피어                   | 포커스 및 선택 영역을 디자인 화면에서 동일한 피어 수준의 다음 개체로 이동 합니다.                                                                                                                                          |
| **Shift+Tab**                                                                           | 이전 피어               | 포커스 및 선택 영역을 디자인 화면의 동일한 피어 수준에서 이전 개체로 이동 합니다.                                                                                                                                      |
| **Alt + Ctrl + Tab**                                                                        | 다음 피어 (포커스)           | 다음 피어와 동일 하지만 포커스 및 선택 영역을 이동 하는 대신 포커스를 이동 합니다.                                                                                                                                                           |
| **Alt + Ctrl + Shift + Tab**                                                                  | 이전 피어 (포커스)       | 이전 피어와 동일 하지만 포커스 및 선택 영역을 이동 하는 대신 포커스를 이동 합니다.                                                                                                                                                       |
| **&lt;**                                                                                | Ascend                      | 계층 구조에서 한 수준 위의 디자인 화면에서 다음 개체로 이동 합니다. 계층 구조에서이 셰이프 위에 셰이프가 없으면 (즉, 개체가 디자인 화면에 직접 배치 됨) 다이어그램이 선택 됩니다. |
| **&gt;**                                                                                | 들어                     | 디자인 화면에서 포함 된 다음 개체로 이동 합니다. 포함 된 개체가 없는 경우이는 작동 하지 않습니다.                                                                              |
| **Ctrl + &lt;**                                                                         | Ascend (포커스)              | Ascend 명령과 동일 하지만 선택 영역 없이 포커스를 이동 합니다.                                                                                                                                                                          |
| **Ctrl + &gt;**                                                                         | 내림차순 (포커스)             | 내림차순 명령과 동일 하지만 선택 영역 없이 포커스를 이동 합니다.                                                                                                                                                                         |
| **Shift + End**                                                                         | 연결 하려면 다음을 수행 합니다.         | 엔터티에서이 엔터티가 연결 된 엔터티로 이동 합니다.                                                                                                                                                               |
| **Del**                                                                                 | 삭제                      | 다이어그램에서 개체 또는 연결선을 삭제 합니다.                                                                                                                                                                                     |
| **기능**                                                                                 | 삽입                      | "스칼라 속성" 구획 헤더 또는 속성 자체가 선택 된 경우 엔터티에 새 속성을 추가 합니다.                                                                                                           |
| **위쪽으로**                                                                               | 다이어그램 위로 스크롤           | 디자인 화면을 현재 표시 되는 디자인 화면 높이의 75%와 같은 단위로 위쪽으로 스크롤합니다.                                                                                                                    |
| **하위 페이지로**                                                                             | 다이어그램 아래로 스크롤         | 디자인 화면을 아래로 스크롤합니다.                                                                                                                                                                                                    |
| **Shift + Pg**                                                                     | 다이어그램 오른쪽으로 스크롤        | 디자인 화면을 오른쪽으로 스크롤합니다.                                                                                                                                                                                            |
| **Shift + Pg 위로**                                                                       | 왼쪽으로 다이어그램 스크롤         | 디자인 화면을 왼쪽으로 스크롤합니다.                                                                                                                                                                                             |
| **F2**                                                                                  | 편집 모드로 전환             | 텍스트 컨트롤에 대 한 편집 모드를 시작 하기 위한 표준 바로 가기 키입니다.                                                                                                                                                               |
| **Shift + F10**                                                                         | 바로 가기 메뉴 표시       | 선택한 항목의 바로 가기 메뉴를 표시 하는 표준 바로 가기 키입니다.                                                                                                                                                          |
| **Ctrl + Shift + 마우스 왼쪽 단추 클릭**  <br/> **컨트롤 + Shift + MouseWheel 앞으로**  | 의미 체계 확대            | 마우스 포인터 아래에 있는 다이어그램 뷰의 영역을 확대 합니다.                                                                                                                                                                 |
| **Ctrl + Shift + 마우스 오른쪽 단추 클릭** <br/> **Ctrl + Shift + MouseWheel 뒤로** | 의미 체계 축소           | 다이어그램 뷰 영역에서 마우스 포인터 아래의 영역을 축소 합니다. 현재 다이어그램 센터에 대해 너무 멀리 축소 하면 다이어그램을 다시 가운데에 배치 합니다.                                                                          |
| **Ctrl + Shift + ' + '** <br/> **컨트롤 + MouseWheel 전달**                        | 확대                     | 다이어그램 뷰의 가운데를 확대 합니다.                                                                                                                                                                                         |
| **Ctrl + Shift + '-'** <br/> **Control + MouseWheel 뒤로**                       | 축소                    | 다이어그램 뷰의 클릭 된 영역을 축소 합니다. 현재 다이어그램 센터에 대해 너무 멀리 축소 하면 다이어그램을 다시 가운데에 배치 합니다.                                                                                            |
| **Ctrl + Shift + 마우스 왼쪽 단추를 눌러 사각형 그리기**                  | 확대/축소 영역                   | 선택한 영역을 중심으로 확대 합니다. Ctrl + Shift 키를 누른 상태에서 커서가 확대경로 변경 되 면 확대할 영역을 정의할 수 있습니다.                        |
| **상황에 맞는 메뉴 키 + ' m '**                                                              | 매핑 정보 창 열기 | 매핑 정보 창을 열어 선택한 엔터티에 대 한 매핑을 편집 합니다.                                                                                                                                                               |

## <a name="mapping-details-window"></a>매핑 정보 창

![매핑 정보 바로 가기](~/ef6/media/mappingdetailsshortcuts.png)

| 바로 가기                  | 작업         | 참고                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **Tab**                   | 컨텍스트 전환 | 주 창 영역과 왼쪽의 도구 모음 사이를 전환 합니다.                                                                     |
| **화살표 키**            | 탐색     | 주 창 영역에서 위쪽 및 아래쪽 행을 이동 하거나 열 전체에 대해 오른쪽에서 왼쪽으로 이동 합니다. 왼쪽의 도구 모음에 있는 단추 사이를 이동 합니다. |
| **Enter** <br/> **스페이스바** | 선택         | 왼쪽의 도구 모음에서 단추를 선택 합니다.                                                                                          |
| **Alt + 아래쪽 화살표**      | 목록 열기      | 드롭다운 목록을 포함 하는 셀이 선택 된 경우 목록을 드롭다운 합니다.                                                                     |
| **Enter**                 | 목록 선택    | 드롭다운 목록에서 요소를 선택 합니다.                                                                                               |
| **Esc**                   | 목록 닫기     | 드롭다운 목록을 닫습니다.                                                                                                              |

## <a name="visual-studio-navigation"></a>Visual Studio 탐색

또한 Entity Framework는 사용자 지정 바로 가기 키가 매핑될 수 있는 많은 작업을 제공 합니다 (기본적으로는 바로 가기가 매핑되지 않음). 이러한 사용자 지정 바로 가기를 만들려면 도구 메뉴를 클릭 한 다음 옵션을 클릭 합니다.  환경에서 키보드를 선택 합니다.  원하는 명령을 선택할 수 있을 때까지 가운데 목록 아래로 스크롤하여 "바로 가기 키 누르기" 텍스트 상자에 바로 가기를 입력 하 고 할당을 클릭 합니다. 가능한 바로 가기는 다음과 같습니다.

| 바로 가기                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus. MicrosoftDataEntityDesignContext 속성. ComplexTypes**        |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddCodeGenerationItem**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddFunctionImport**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddEnumType**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext 속성**                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext.**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext.**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext.**               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ScalarProperty**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNewDiagram**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext 다이어그램**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ConverttoEnum**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. CollapseAll**                     |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ExpandAll**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ExportasImage**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext 다이어그램**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. GenerateDatabasefromModel**               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. GoToDefinition**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ShowGrid**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. SnaptoGrid**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. IncludeRelated**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MappingDetails**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext 브라우저**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MoveDiagramstoSeparateFile**              |
| **OtherContextMenus. MicrosoftDataEntityDesignContext 속성입니다.**                     |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Down5**                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ToBottom**                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext 속성. ToTop**                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext 속성입니다.**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Up5**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                        |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MovetoNewComplexType**           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. 이름 바꾸기**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. RemovefromDiagram**                       |
| **OtherContextMenus MicrosoftDataEntityDesignContext. 이름 바꾸기**                                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ScalarPropertyFormat**        |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ScalarPropertyFormat Andtype** |
| **OtherContextMenus. MicrosoftDataEntityDesignContext를 선택 합니다.**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext를 선택 합니다.**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. 속성을 선택 합니다.**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext를 선택 합니다.**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. SelectAll**                               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. SelectAssociation**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext Win다이어그램**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext Winmodelbrowser**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. StoredProcedureMapping**                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. TableMapping**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. UpdateModelfromDatabase**                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. 10**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. 100**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. 25**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext**                             |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ZoomIn**                             |
| **OtherContextMenus MicrosoftDataEntityDesignContext. 확대/축소**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. 확대/축소**                          |
| **EntityDataModelBrowser**                                                                |
| **EntityDataModelMappingDetails**                                                         |
