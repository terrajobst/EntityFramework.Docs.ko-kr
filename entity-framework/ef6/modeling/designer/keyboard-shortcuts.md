---
title: Entity Framework 디자이너 바로 가기 키-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
caps.latest.revision: 3
ms.openlocfilehash: a4ed95647872949d9e34a6bb5d83d5d6119b0022
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122818"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Entity Framework 디자이너 바로 가기 키
이 페이지에는 Entity Framework Tools for Visual Studio의 다양 한 화면에서 사용할 수 있는 키보드 shorcuts 목록을 제공 합니다.

## <a name="adonet-entity-data-model-wizard"></a>ADO.NET 엔터티 데이터 모델 마법사

### <a name="step-one-choose-model-contents"></a>1 단계: Model 콘텐츠 선택

![WizardOne](~/ef6/media/wizardone.png)

| 바로 가기  | 작업                                                     | 노트                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **Alt + n** | 다음 화면으로 이동                                        | 모델 콘텐츠의 모든 선택 항목에 대해 사용할 수 없습니다. |
| **Alt + f** | 마법사 완료                                              | 모델 콘텐츠의 모든 선택 항목에 대해 사용할 수 없습니다. |
| **Alt + w** | "어떤 모델에 포함 될?"에 포커스를 전환 합니다. 창입니다. |                                                     |

### <a name="step-two-choose-your-connection"></a>2 단계: 연결 선택

![WizardTwo](~/ef6/media/wizardtwo.png)

| 바로 가기  | 작업                                                     | 노트                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **Alt + n** | 다음 화면으로 이동                                        |                                                         |
| **Alt + p** | 이전 화면으로 이동                                    |                                                         |
| **Alt + w** | "어떤 모델에 포함 될?"에 포커스를 전환 합니다. 창입니다. |                                                         |
| **Alt + c** | "연결 속성" 창 열기                    | 새 데이터베이스 연결의 정의 허용합니다. |
| **Alt + e** | 연결 문자열에서 중요 한 데이터를 제외 합니다.          |                                                         |
| **Alt + i** | 연결 문자열에 중요 한 데이터를 포함 합니다.            |                                                         |
| **Alt + s** | "App.Config의 연결 설정 저장" 옵션을 설정/해제 |                                                         |

### <a name="step-three-choose-your-version"></a>3 단계: 버전 선택

![WizardThree](~/ef6/media/wizardthree.png)

| 바로 가기  | 작업                                             | 노트                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **Alt + n** | 다음 화면으로 이동                                |                                                                                       |
| **Alt + p** | 이전 화면으로 이동                            |                                                                                       |
| **Alt + w** | Entity Framework 버전 선택 항목에 포커스를 전환 합니다. | 프로젝트에서 사용 하기 위해 Entity Framework의 다른 버전을 지정할 수 있습니다. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>4 단계: 데이터베이스 개체 및 설정 선택

![WizardFour](~/ef6/media/wizardfour.png)

| 바로 가기  | 작업                                                                                    | 노트                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **Alt + f** | 마법사 완료                                                                             |                                                                     |
| **Alt + p** | 이전 화면으로 이동                                                                   |                                                                     |
| **Alt + w** | 데이터베이스 개체 선택 창에 포커스를 전환 합니다.                                            | 엔지니어링 역방향 되도록 데이터베이스 개체를 지정할 수 있습니다.    |
| **Alt + s** | 설정/해제는 "복수화 또는 생성 된 개체 이름을 단 수 화" 옵션                       |                                                                     |
| **Alt + k** | "모델에서 외래 키 열 포함" 옵션을 설정/해제                              | 모델 콘텐츠의 모든 선택 항목에 대해 사용할 수 없습니다.                 |
| **Alt + i** | "가져오기 엔터티 모델에 저장된 프로시저 및 함수를 선택" 옵션을 설정/해제 | 모델 콘텐츠의 모든 선택 항목에 대해 사용할 수 없습니다.                 |
| **Alt + m** | "모델 Namespace" 텍스트 필드에 포커스를 전환 합니다.                                        | 모델 콘텐츠의 모든 선택 항목에 대해 사용할 수 없습니다.                 |
| **스페이스바** | 요소에 대해 선택 설정/해제                                                               | 요소 자식이 있으면 모든 자식 요소를 설정/해제할 수도 |
| **왼쪽**  | 자식 트리 축소                                                                       |                                                                     |
| **오른쪽** | 하위 트리를 확장 합니다.                                                                         |                                                                     |
| **등록**    | 트리에서 이전 요소로 이동                                                      |                                                                     |
| **아래로**  | 트리에서 다음 요소로 이동                                                          |                                                                     |

## <a name="ef-designer-surface"></a>EF 디자이너 화면

![DesignerSurface](~/ef6/media/designersurface.png)

| 바로 가기                                                                                | 작업                      | 노트                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **공간을 입력 하거나**                                                                         | 선택 영역 전환            | 포커스가 있는 개체 선택 영역을 전환합니다.                                                                                                                                                                                         |
| **Esc**                                                                                 | 선택 취소            | 현재 선택을 취소합니다.                                                                                                                                                                                                      |
| **Ctrl + A**                                                                            | 모두 선택                  | 디자인 화면에서 모든 셰이프를 선택합니다.                                                                                                                                                                                       |
| **위쪽 화살표**                                                                            | 위로 이동                     | 선택한 그리드 증가 하는 엔터티를 이동 합니다. <br/> 목록에서 작업 하는 경우 이전 형제 하위 필드를 이동 합니다.                                                                                                                            |
| **아래쪽 화살표**                                                                          | 아래로 이동                   | 선택한 엔터티 한 모눈 아래로 이동 합니다. <br/> 목록에서 작업 하는 경우 다음 형제 하위 필드를 이동 합니다.                                                                                                                              |
| **왼쪽 화살표**                                                                          | 왼쪽으로 이동                   | 선택한 엔터티 왼쪽된 한 모눈을 이동 합니다. <br/> 목록에서 작업 하는 경우 이전 형제 하위 필드를 이동 합니다.                                                                                                                          |
| **오른쪽 화살표**                                                                         | 오른쪽으로 이동                  | 선택한 엔터티 오른쪽으로 한 모눈을 이동 합니다. <br/> 목록에서 작업 하는 경우 다음 형제 하위 필드를 이동 합니다.                                                                                                                             |
| **Shift + 왼쪽된 화살표**                                                                  | 왼쪽 크기 셰이프             | 한 모눈에서 선택한 엔터티의 너비를 줄입니다.                                                                                                                                                                     |
| **Shift + 오른쪽 화살표**                                                                 | 오른쪽 크기 셰이프            | 선택한 엔터티의 너비 그리드 증분만큼 늘립니다.                                                                                                                                                                   |
| **Home**                                                                                | 첫 번째 피어                  | 포커스를 이동 하 고 같은 피어 수준의 디자인 화면에서 첫 번째 개체를 선택 합니다.                                                                                                                                         |
| **End**                                                                                 | 마지막 피어                   | 포커스를 이동 하 고 같은 피어 수준의 디자인 화면에 있는 마지막 개체를 선택 합니다.                                                                                                                                          |
| **Ctrl + Home**                                                                         | 첫 번째 피어 (포커스 내)          | 포커스 및 선택 영역을 이동 하는 대신 포커스를 이동 하지만 첫 번째 피어와 동일 합니다.                                                                                                                                                          |
| **Ctrl + End**                                                                          | 마지막 피어 (포커스 내)           | 마지막 동일에 피어 링 하지만 포커스 및 선택 영역을 이동 하는 대신 포커스를 이동 합니다.                                                                                                                                                           |
| **Tab**                                                                                 | 다음 피어                   | 포커스를 이동 하 고 같은 피어 수준의 디자인 화면에서 다음 개체를 선택 합니다.                                                                                                                                          |
| **Shift+Tab**                                                                           | 이전 피어               | 포커스를 이동 하 고 같은 피어 수준의 디자인 화면에서 이전 개체를 선택 합니다.                                                                                                                                      |
| **Alt + Ctrl + Tab**                                                                        | 다음 피어 (포커스 내)           | 다음 동일에 피어 링 하지만 포커스 및 선택 영역을 이동 하는 대신 포커스를 이동 합니다.                                                                                                                                                           |
| **Alt + Ctrl + Shift + Tab**                                                                  | 이전 피어 (포커스 내)       | 포커스 및 선택 영역을 이동 하는 대신 포커스를 이동 하지만 이전 피어와 동일 합니다.                                                                                                                                                       |
| **&lt;**                                                                                | 위로                      | 다음 개체를 디자인 화면에서 한 수준 높은 계층으로 이동 합니다. 계층 구조에서이 셰이프 위쪽 셰이프가 없는 경우 (즉, 개체를 배치 디자인 화면에서 직접), 다이어그램을 선택 합니다. |
| **&gt;**                                                                                | 내림차순                     | 이 계층에서 아래 디자인 화면의 한 수준에는 다음 포함 된 개체를 이동 합니다. 포함 된 개체가 없는 경우이 작동 하지 않습니다.                                                                              |
| **Ctrl + &lt;**                                                                         | 위로 (포커스 내)              | 선택 하지 않고 포커스를 이동 하지만 명령을 위로 동일 합니다.                                                                                                                                                                          |
| **Ctrl + &gt;**                                                                         | 내림차순 (포커스 내)             | 선택 하지 않고 포커스를 이동 하지만 명령인을 내림차순 동일 합니다.                                                                                                                                                                         |
| **Shift + End**                                                                         | 연결에 따라         | 엔터티의이 엔터티에에 연결 되는 엔터티를 이동 합니다.                                                                                                                                                               |
| **Del**                                                                                 | 삭제                      | 다이어그램에서 개체 또는 커넥터를 삭제 합니다.                                                                                                                                                                                     |
| **기능**                                                                                 | Insert                      | "스칼라 속성" 구획 헤더 또는 자체 속성을 선택 하면 새 속성을 엔터티에 추가 합니다.                                                                                                           |
| **Pg 등록**                                                                               | 위로 스크롤 다이어그램           | 현재 표시 된 디자인 화면의 높이의 75%와 동일한 증분으로, 디자인 화면을 스크롤합니다.                                                                                                                    |
| **아래로 Pg**                                                                             | 아래로 스크롤 다이어그램         | 디자인 화면을 아래로 스크롤합니다.                                                                                                                                                                                                    |
| **Shift + 아래쪽 Pg**                                                                     | 오른쪽 스크롤 다이어그램        | 디자인 화면을 오른쪽으로 스크롤합니다.                                                                                                                                                                                            |
| **Shift + 위쪽 Pg**                                                                       | 왼쪽 스크롤 다이어그램         | 디자인 화면을 왼쪽으로 스크롤합니다.                                                                                                                                                                                             |
| **F2**                                                                                  | 편집 모드로 전환             | 텍스트 컨트롤에 대 한 편집 모드가 표준 키보드 바로 가기.                                                                                                                                                               |
| **Shift+f10**                                                                         | 바로 가기 메뉴 표시       | 선택한 항목의 바로 가기 메뉴를 표시 하기 위한 표준 키보드 바로 가기.                                                                                                                                                          |
| **컨트롤 + Shift + 마우스 왼쪽 단추를 클릭합니다**  <br/> **컨트롤 + Shift + 마우스 휠을 앞으로**  | 의미 체계 확대            | 마우스 포인터 아래에 있는 다이어그램 뷰의 영역에 확대 됩니다.                                                                                                                                                                 |
| **컨트롤 + Shift + 마우스 오른쪽 단추로 클릭** <br/> **컨트롤 + Shift + 마우스 휠을 이전 버전과** | 의미 체계 축소           | 마우스 포인터 아래에 있는 다이어그램 뷰의 영역에서 축소합니다. 다시 현재 다이어그램 센터용 멀리 축소 하면 다이어그램 센터.                                                                          |
| **제어 + Shift + '+'** <br/> **컨트롤 + 마우스 휠을 앞으로**                        | 확대                     | 다이어그램 뷰의 가운데로 확대합니다.                                                                                                                                                                                         |
| **제어 + Shift + '-'** <br/> **컨트롤 + MouseWheel 이전 버전과**                       | 축소                    | 다이어그램 보기를 클릭 한 영역에서 축소합니다. 다시 현재 다이어그램 센터용 멀리 축소 하면 다이어그램 센터.                                                                                            |
| **제어 + Shift + 마우스 왼쪽된 단추를 사용 하 여 사각형 그리기**                  | 영역을 확대/축소                   | 선택한 영역에서 중심 확대 합니다. 컨트롤 + Shift 키를 누르고 있을 때 커서 영역으로 확대/축소를 정의할 수 있는 돋보기를으로 변경 되는지 표시 됩니다.                        |
| **상황에 맞는 메뉴 키 + 여기 '**                                                              | 매핑 정보 창 열기 | 선택한 엔터티에 대 한 매핑을 편집할 매핑 정보 창을 시작                                                                                                                                                               |

## <a name="mapping-details-window"></a>매핑 정보 창

![MappingDetailsShortcuts](~/ef6/media/mappingdetailsshortcuts.png)

| 바로 가기                  | 작업         | 노트                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **Tab**                   | 컨텍스트 전환 | 주 창 영역을 왼쪽의 도구 모음 사이의 스위치                                                                     |
| **화살표 키**            | 탐색     | 주 창 영역에서 열에 걸쳐 행 또는 오른쪽 및 왼쪽 위/아래로 이동 합니다. 왼쪽 도구 모음 단추 간에 이동 합니다. |
| **Enter** <br/> **스페이스바** | 선택         | 왼쪽 도구 모음에서 단추를 선택합니다.                                                                                          |
| **Alt + 아래쪽 화살표**      | 목록 열기      | 드롭다운 목록에 있는 셀을 선택한 경우 목록을 드롭다운 합니다.                                                                     |
| **Enter**                 | 목록 선택    | 드롭다운 목록에서에서 요소를 선택합니다.                                                                                               |
| **Esc**                   | 목록 닫기     | 드롭다운 목록 닫습니다.                                                                                                              |

## <a name="visual-studio-navigation"></a>Visual Studio 탐색

Entity Framework에는 또한 몇 가지 사용자 지정 바로 가기 키를 매핑할 수 있는 작업을 제공 (기본적으로 매핑되는 바로 가기가 없습니다). 이러한 사용자 지정 바로 가기 키를 만들려면 도구 메뉴에서 다음 옵션을 클릭 합니다.  환경에서 키보드를 선택 합니다.  중간 원하는 명령을 선택, "바로 가기 키 누르기" 텍스트 상자에 바로 가기를 입력 및 클릭 될 때까지 목록 아래로 스크롤한 할당 합니다. 가능한 바로 가기는 다음과 같습니다.

| 바로 가기                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Add.ComplexProperty.ComplexTypes**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddCodeGenerationItem**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddFunctionImport**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.AddEnumType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Association**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexProperty**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.FunctionImport**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Inheritance**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.NavigationProperty**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ScalarProperty**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNewDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddtoDiagram**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Close**                                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Collapse**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ConverttoEnum**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.CollapseAll**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExpandAll**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExportasImage**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.LayoutDiagram**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Edit**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.EntityKey**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Expand**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.FunctionImportMapping**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GenerateDatabasefromModel**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GoToDefinition**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.ShowGrid**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.SnaptoGrid**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.IncludeRelated**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MappingDetails**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ModelBrowser**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveDiagramstoSeparateFile**              |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down5**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToBottom**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToTop**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up5**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MovetonewDiagram**                        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Open**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.MovetoNewComplexType**           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.Rename**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.RemovefromDiagram**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Rename**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayName**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayNameandType** |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.BaseType**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Property**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Subtype**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAll**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.TableMapping**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Validate**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.10**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.100**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.125**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.150**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.200**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.25**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.300**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.33**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.400**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.50**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.66**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.75**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.Custom**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomIn**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomOut**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomtoFit**                          |
| **View.EntityDataModelBrowser**                                                                |
| **View.EntityDataModelMappingDetails**                                                         |
