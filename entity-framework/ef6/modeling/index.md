---
title: 모델 만들기 - EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 4890228E-CEA1-4595-B8AD-CA81253F8767
ms.openlocfilehash: bd9843a93121f53518a307c9d2d43b68ae03369c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413468"
---
# <a name="creating-a-model"></a>모델 만들기

EF 모델은 애플리케이션 클래스 및 속성이 데이터베이스 테이블과 열에 매핑하는 방식에 대한 세부 정보를 저장합니다. EF 모델을 만드는 방법에는 크게 두 가지가 있습니다.

- **Code First 사용**: 개발자는 모델을 지정하는 코드를 작성합니다. EF는 런타임에 개발자가 제공한 엔터티 클래스 및 추가 모델 구성을 기반으로 모델과 매핑을 생성합니다.

- **EF 디자이너 사용**: 개발자는 EF 디자이너를 사용하여 모델을 지정하는 상자와 선을 그립니다. 그 결과로 생성되는 모델은 EDMX 확장을 사용하여 파일에 XML로 저장됩니다. 애플리케이션의 도메인 개체는 일반적으로 개념적 모델에서 자동으로 생성됩니다.

## <a name="ef-workflows"></a>EF 워크플로

두 방법 모두 기존 데이터베이스를 대상으로 지정하거나 새 데이터베이스를 만드는 데 사용할 수 있으며, 그 결과로 4가지 워크플로를 얻게 됩니다.
어떤 것이 가장 적합한지 알아보세요.  

|                                           | 코드만 작성하려 합니다...                                                                                                                   | 디자이너를 사용하고 싶습니다...                                                                                                                        |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| **새 데이터베이스를 만들려는 경우**          | [**Code First**를 사용하여 코드에서 모델을 정의한 다음, 데이터베이스를 생성합니다.](~/ef6/modeling/code-first/workflows/new-database.md)           | [**Model First**를 사용하여 상자와 선을 이용해 모델을 정의한 다음, 데이터베이스를 생성합니다.](~/ef6/modeling/designer/workflows/model-first.md)   |
| **기존 데이터베이스에 액세스해야 하는 경우** | [**Code First**를 사용하여 기존 데이터베이스에 매핑하는 코드 기반 모델을 만듭니다.](~/ef6/modeling/code-first/workflows/existing-database.md) | [**Database First**를 사용하여 기존 데이터베이스에 매핑하는 상자 및 선 모델을 만듭니다.](~/ef6/modeling/designer/workflows/database-first.md) |

### <a name="watch-the-video-what-ef-workflow-should-i-use"></a>비디오 보기: 어떤 EF 워크플로를 사용해야 하나요?

이 짧은 비디오에서는 워크플로 간 차이점과 적합한 워크플로를 찾는 방법을 설명합니다.

**작성자**: [Rowan Miller](https://romiller.com/)

![Which Workflow Thumb](../media/whichworkflow-thumb.png) [WMV](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) | [MP4](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v) | [WMV(ZIP)](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip)

비디오를 시청한 후에도 EF 디자이너와 Code First 중 무엇을 사용할지 결정하지 못했다면 둘 다 공부해 보세요.

## <a name="a-look-under-the-hood"></a>내부 살펴보기

Code First와 EF 디자이너 중에서 무엇을 사용하든, EF 모델에는 항상 다음과 같은 여러 구성 요소가 있습니다.

- 바로 애플리케이션의 도메인 개체 또는 엔터티 형식 자체. 개체 레이어라고도 합니다.

- 도메인 관련 엔터티 형식 및 관계로 구성되며 [엔터티 데이터 모델](~/ef6/resources/glossary.md#entity-data-model)을 사용하여 설명되는 개념적 모델. 이 레이어는 _conceptual_을 의미하는 "C"로 부르기도 합니다.

- 데이터베이스에 정의된 테이블, 열 및 관계를 나타내는 스토리지 모델. 이 레이어는 _storage_를 의미하는 "S"로 부르기도 합니다.  

- 개념적 모델과 데이터베이스 스키마 간의 매핑. 이 매핑을 "C-S" 매핑이라고도 합니다.

EF의 매핑 엔진은 "C-S" 매핑을 활용하여 만들기, 읽기, 업데이트, 삭제 등 엔터티에 대한 작업을 데이터베이스 테이블에 대한 동등한 작업으로 변환합니다.

개념적 모델과 애플리케이션 개체 간의 매핑을 "O-C" 매핑이라고 부르기도 합니다. "C-S" 매핑과 비교하면, "O-C" 매핑은 암시적인 일대일 매핑입니다. 개념적 모델에 정의된 엔터티, 속성 및 관계가 .NET 개체의 셰이프 및 형식과 일치해야 합니다. EF4 이상부터 계층 레이어는 EF에 대한 종속성 없이 속성이 있는 간단한 개체로 구성될 수 있습니다. 이를 보통 POCO(Plain Old CLR Object)라고 부르며 형식 및 속성 매핑은 이름 일치 규칙에 따라 수행됩니다. 기존의 EF 3.5에서는 엔터티가 EntityObject 클래스에서 파생되어야 하고 "O-C" 매핑을 구현하기 위한 EF 특성을 갖고 있어야 하는 등 개체 레이어와 관련된 제한이 있었습니다.
