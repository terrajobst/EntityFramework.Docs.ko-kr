---
title: 디자이너 코드 생성 템플릿 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 56e00fa2-f9f0-48b3-8006-f8266ca7e74b
ms.openlocfilehash: e4e99a86e7c273682c85eba06042af9a2a837d12
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413288"
---
# <a name="designer-code-generation-templates"></a>디자이너 코드 생성 템플릿
Entity Framework Designer를 사용하여 모델을 만들면 클래스와 파생된 컨텍스트가 자동으로 생성됩니다. 기본 코드 생성 외에도 생성된 코드를 사용자 지정하는 데 사용할 수 있는 다양한 템플릿을 제공합니다. 이러한 템플릿은 T4 텍스트 템플릿으로 제공되므로 필요한 경우 템플릿을 사용자 지정할 수 있습니다.

기본적으로 생성되는 코드는 모델을 만드는 Visual Studio의 버전에 따라 다릅니다.

-   Visual Studio 2012 및 2013에서 만든 모델은 간단한 POCO 엔터티 클래스 및 간소화된 DbContext에서 파생된 컨텍스트를 생성합니다.
-   Visual Studio 2010에서 만든 모델은 EntityObject에서 파생된 엔터티 클래스 및 ObjectContext에서 파생된 컨텍스트를 생성합니다.
  > [!NOTE]    
  > 모델이 추가되면 DbContext 생성기 템플릿으로 전환하는 것이 좋습니다.

이 페이지에서는 사용 가능한 템플릿에 대해 설명하고, 템플릿을 모델에 추가하기 위한 지침을 제공합니다.

## <a name="available-templates"></a>사용 가능한 템플릿

Entity Framework 팀에서 제공하는 템플릿은 다음과 같습니다.

### <a name="dbcontext-generator"></a>DbContext 생성기

이 템플릿은 간단한 POCO 엔터티 클래스와 EF6를 사용하여 DbContext에서 파생되는 컨텍스트를 생성합니다.
아래에 나열된 다른 템플릿 중 하나를 사용해야 하는 이유가 없는 한 이 템플릿을 사용하는 것이 좋습니다.
최신 버전의 Visual Studio(Visual Studio 2013 이상)를 사용하는 경우 기본적으로 가져오는 코드 생성 템플릿이기도 합니다. 새 모델을 만들 때 이 템플릿은 기본적으로 사용되고 T4 파일(.tt)은 .edmx 파일 아래에 중첩됩니다.

#### <a name="older-versions-of-visual-studio"></a>이전 버전의 Visual Studio
- **Visual Studio 2012:** **EF 6.x DbContext 생성기** 템플릿을 얻으려면 최신 **Visual Studio용 Entity Framework Tools**를 설치해야 합니다. 자세한 내용은 [Entity Framework 가져오기](~/ef6/fundamentals/install.md) 페이지를 참조하세요.
- **Visual Studio 2010:** **EF 6.x DbContext 생성기** 템플릿은 Visual Studio 2010에서 사용할 수 없습니다.

#### <a name="dbcontext-generator-for-ef-5x"></a>EF 5.x용 DbContext 생성기

이전 버전의 EntityFramework NuGet 패키지(주 버전이 5인 패키지)를 사용하는 경우 **EF 5.x DbContext 생성기** 템플릿을 사용해야 합니다.

Visual Studio 2013 또는 2012를 사용하는 경우 이 템플릿은 이미 설치되어 있습니다.

Visual Studio 2010을 사용하는 경우 템플릿을 추가할 때 **온라인** 탭을 선택하여 Visual Studio 갤러리에서 템플릿을 다운로드해야 합니다. 또는 미리 Visual Studio 갤러리에서 템플릿을 직접 설치할 수도 있습니다. 템플릿은 이후 버전의 Visual Studio에 포함되어 있으므로 갤러리의 버전은 Visual Studio 2010에만 설치할 수 있습니다.

- [C#용 EF 5.x DbContext 생성기](https://visualstudiogallery.msdn.microsoft.com/da740968-02f9-42a9-9ee4-1a9a06d896a2)
- [C# 웹 사이트용 EF 5.x DbContext 생성기](https://visualstudiogallery.msdn.microsoft.com/5d01a981-91b8-492c-b42c-c771c3f31e03)
- [VB.NET용 EF 5.x DbContext 생성기](https://visualstudiogallery.msdn.microsoft.com/875c882d-333e-455a-8dae-5353510527dd?src=featured)
- [VB.NET 웹 사이트용 EF 5.x DbContext 생성기](https://visualstudiogallery.msdn.microsoft.com/d4d7d4cd-c2d0-43e6-8944-12f6ff8f2614)

#### <a name="dbcontext-generator-for-ef-4x"></a>EF 4.x용 DbContext 생성기

이전 버전의 EntityFramework NuGet 패키지(주 버전이 4인 패키지)를 사용하는 경우 **EF 4.x DbContext 생성기** 템플릿을 사용해야 합니다. 이 템플릿은 템플릿을 추가할 때 **온라인** 탭에서 찾을 수 있거나. 미리 Visual Studio 갤러리에서 템플릿을 직접 설치할 수도 있습니다.

- [C#용 EF 4.x DbContext 생성기](https://visualstudiogallery.msdn.microsoft.com/7812b04c-db36-4817-8a84-e73c452410a2)
- [C# 웹 사이트용 EF 4.x DbContext 생성기](https://visualstudiogallery.msdn.microsoft.com/de0e9bc6-e86a-4448-8a2e-a1260a53203e)
- [VB.NET용 EF 4.x DbContext 생성기](https://visualstudiogallery.msdn.microsoft.com/73679ae5-e358-4e76-a538-c7b5e04ac073)
- [VB.NET 웹 사이트용 EF 4.x DbContext 생성기](https://visualstudiogallery.msdn.microsoft.com/86f5a660-306e-4831-840c-2e4ee7474a92)

### <a name="entityobject-generator"></a>EntityObject 생성기

이 템플릿은 EntityObject에서 파생된 엔터티 클래스 및 ObjectContext에서 파생된 컨텍스트를 생성합니다.

> [!NOTE]
> DbContext 생성기를 사용하는 것이 좋습니다.

DbContext 생성기는 이제 새 애플리케이션에 권장되는 템플릿입니다. DbContext 생성기는 더 간단한 DbContext API를 활용합니다. EntityObject 생성기는 기존 애플리케이션을 지원하는 데 계속 사용할 수 있습니다.

**Visual Studio 2010, 2012 및 2013**

템플릿을 추가할 때 **온라인** 탭을 선택하여 Visual Studio 갤러리에서 템플릿을 다운로드해야 합니다. 또는 미리 Visual Studio 갤러리에서 템플릿을 직접 설치할 수도 있습니다.

- [C#용 EF 6.x EntityObject 생성기](https://visualstudiogallery.msdn.microsoft.com/66612113-549c-4a9e-a14a-f629ceb3f89a)
- [C# 웹 사이트용 EF 6.x EntityObject 생성기](https://visualstudiogallery.msdn.microsoft.com/076140f3-6dbe-451f-a0e0-16b6d2bd8996)
- [VB.NET용 EF 6.x EntityObject 생성기](https://visualstudiogallery.msdn.microsoft.com/ff479d55-2c85-43c5-a4d6-21cd659435ea)
- [VB.NET 웹 사이트용 EF 6.x EntityObject 생성기](https://visualstudiogallery.msdn.microsoft.com/668e2b92-c142-4da2-8e60-866c6346fc6a)

**EF 5.x용 EntityObject 생성기**


Visual Studio 2012 또는 2013을 사용하는 경우 템플릿을 추가할 때 **온라인** 탭을 선택하여 Visual Studio 갤러리에서 템플릿을 다운로드해야 합니다. 또는 미리 Visual Studio 갤러리에서 템플릿을 직접 설치할 수도 있습니다. 템플릿은 이후 버전의 Visual Studio 2010에 포함되어 있으므로 갤러리의 버전은 Visual Studio 2012 및 2013에만 설치할 수 있습니다.

- [C#용 EF 5.x EntityObject 생성기](https://visualstudiogallery.msdn.microsoft.com/1da40393-b5ec-404a-a000-6a7e6e911339)
- [C# 웹 사이트용 EF 5.x EntityObject 생성기](https://visualstudiogallery.msdn.microsoft.com/94b48556-fcf0-4b9b-8615-20f9066ae9ac)
- [VB.NET용 EF 5.x EntityObject 생성기](https://visualstudiogallery.msdn.microsoft.com/92c0129e-40dc-488c-a836-7e30846dfb30)
- [VB.NET 웹 사이트용 EF 5.x EntityObject 생성기](https://visualstudiogallery.msdn.microsoft.com/5dd7f75c-8c98-4eb7-b4bc-06f0d0b03b41)

템플릿을 편집할 필요가 없는 ObjectContext 코드 생성을 원하는 경우 [EntityObject 코드 생성으로 되돌릴 수 있습니다](~/ef6/modeling/designer/codegen/legacy-objectcontext.md).

Visual Studio 2010을 사용하는 경우 이 템플릿은 이미 설치되어 있습니다. Visual Studio 2010에서 새 모델을 만드는 경우 이 템플릿이 기본적으로 사용되지만 .tt 파일은 프로젝트에 포함되지 않습니다. 템플릿을 사용자 지정하려면 프로젝트에 이 템플릿을 추가해야 합니다.

### <a name="self-tracking-entities-ste-generator"></a>STE(자체 추적 엔터티) 생성기

이 템플릿은 자체 추적 엔터티 클래스와 ObjectContext에서 파생되는 컨텍스트를 생성합니다. EF 애플리케이션에서 컨텍스트는 엔터티의 변경 내용을 추적합니다. 그러나 N 계층 시나리오의 경우 엔터티를 수정하는 계층에서는 컨텍스트를 사용하지 못할 수도 있습니다. 자체 추적 엔터티를 사용하면 모든 계층의 변경 내용을 추적할 수 있습니다. 자세한 내용은 [자체 추적 엔터티](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/index.md)를 참조하세요.

> [!NOTE]
> STE 템플릿은 권장되지 않습니다.

STE 템플릿은 새 애플리케이션에서 더 이상 사용하지 않는 것이 좋으며, 기존 애플리케이션을 지원하기 위해 계속 사용할 수 있습니다. N 계층 시나리오에 권장되는 다른 옵션은 [연결이 중단된 엔터티 문서](~/ef6/fundamentals/disconnected-entities/index.md)를 참조하세요.

> [!NOTE]
> EF 6.x 버전에는 STE 템플릿이 없습니다.


> [!NOTE]
> Visual Studio 2013 버전에는 STE 템플릿이 없습니다.

### <a name="visual-studio-2012"></a>Visual Studio 2012

Visual Studio 2012를 사용하는 경우 템플릿을 추가할 때 **온라인** 탭을 선택하여 Visual Studio 갤러리에서 템플릿을 다운로드해야 합니다. 또는 미리 Visual Studio 갤러리에서 템플릿을 직접 설치할 수도 있습니다. 템플릿은 이후 버전의 Visual Studio 2010에 포함되어 있으므로 갤러리의 버전은 Visual Studio 2012에만 설치할 수 있습니다.

- [C#용 EF 5.x STE 생성기](https://visualstudiogallery.msdn.microsoft.com/a3ac10a5-9365-4096-bb58-d9a1ba71db8f)
- [C# 웹 사이트용 EF 5.x STE 생성기](https://visualstudiogallery.msdn.microsoft.com/1b55ab82-eeb4-47ba-8d35-3c7c8b5f5a8c)
- [VB.NET용 EF 5.x STE 생성기](https://visualstudiogallery.msdn.microsoft.com/1ba8c6a3-44e9-4e1f-b21e-596f3168474b)
- [VB.NET 웹 사이트용 EF 5.x STE 생성기](https://visualstudiogallery.msdn.microsoft.com/a9fd5f0a-9af4-4e32-9c09-0e057072152e)

#### <a name="visual-studio-2010"></a>Visual Studio 2010**

Visual Studio 2010을 사용하는 경우 이 템플릿은 이미 설치되어 있습니다.

### <a name="poco-entity-generator"></a>POCO 엔터티 생성기

이 템플릿은 POCO 엔터티 클래스와 ObjectContext에서 파생되는 컨텍스트를 생성합니다.

> [!NOTE]
> DbContext 생성기를 사용하는 것이 좋습니다.

DbContext 생성기는 이제 새 애플리케이션에서 POCO 클래스를 생성하는 데 권장되는 템플릿입니다. DbContext 생성기는 새 DbContext API를 활용하고, 더 간단한 POCO 클래스를 생성할 수 있습니다. POCO 엔터티 생성기는 기존 애플리케이션을 지원하는 데 계속 사용할 수 있습니다.

> [!NOTE]
> EF 5.x 또는 EF 6.x 버전에는 STE 템플릿이 없습니다.

> [!NOTE]
> Visual Studio 2013 버전에는 POCO 템플릿이 없습니다.

#### <a name="visual-studio-2012-amp-visual-studio-2010"></a>Visual Studio 2012 및 Visual Studio 2010

템플릿을 추가할 때 **온라인** 탭을 선택하여 Visual Studio 갤러리에서 템플릿을 다운로드해야 합니다. 또는 미리 Visual Studio 갤러리에서 템플릿을 직접 설치할 수도 있습니다.

- [C#용 EF 4.x POCO 생성기](https://visualstudiogallery.msdn.microsoft.com/23df0450-5677-4926-96cc-173d02752313)
- [C# 웹 사이트용 EF 4.x POCO 생성기](https://visualstudiogallery.msdn.microsoft.com/fe568da5-aa1a-4178-a2a5-48813c707a7f)
- [VB.NET용 EF 4.x POCO 생성기](https://visualstudiogallery.msdn.microsoft.com/53ecbded-8936-4299-ab04-1e44e5489752)
- [VB.NET 웹 사이트용 EF 4.x POCO 생성기](https://visualstudiogallery.msdn.microsoft.com/463c5aca-05ad-4cdb-910b-2e4f83269e34)

### <a name="what-are-the-web-sites-templates"></a>"웹 사이트" 템플릿이란?

"웹 사이트"(예: **C\#용 EF 5.x DbContext 생성기 웹 사이트**) 템플릿은 **파일 -&gt; 새로 만들기 -&gt; 웹 사이트...** 를 통해 만든 [웹 사이트] 프로젝트에서 사용됩니다. 이러한 템플릿은 **파일 -&gt; 새로 만들기 -&gt; 프로젝트...** 를 통해 만든 [웹 애플리케이션](표준 템플릿 사용)과 다릅니다. Visual Studio의 항목 템플릿 시스템에 필요하므로 별도의 템플릿이 제공됩니다.

## <a name="using-a-template"></a>템플릿 사용

코드 생성 템플릿 사용을 시작하려면 EF Designer의 디자인 화면에서 빈 곳을 마우스 오른쪽 단추로 클릭하고, **코드 생성 항목 추가...** 를 선택합니다.

![코드 생성 항목 추가](~/ef6/media/add-code-gen-item.png)

사용하려는 템플릿이 이미 설치되어 있거나 Visual Studio에 포함되어 있는 경우 왼쪽 메뉴의 **코드** 또는 **데이터** 섹션에서 사용할 수 있습니다.

![설치됨](~/ef6/media/installed.png)

템플릿이 아직 설치되지 않은 경우 왼쪽 메뉴에서 **온라인**을 선택하고 원하는 템플릿을 검색합니다.

![검색](~/ef6/media/search.png) 

Visual Studio 2012를 사용하는 경우 새 .tt 파일은 .edmx 파일 아래에 중첩됩니다.*

> [!NOTE]
> Visual Studio 2012에서 만든 모델의 경우 기본 코드 생성에 사용되는 템플릿을 삭제해야 합니다. 그렇지 않으면 중복 클래스와 컨텍스트가 생성됩니다. 기본 파일은 **&lt;모델 이름&gt;.tt** 및 **&lt;모델 이름&gt;.context.tt**입니다. 

![VS2012 템플릿](~/ef6/media/vs2012-templates.png)

Visual Studio 2010을 사용하는 경우 tt 파일은 프로젝트에 직접 추가됩니다.  

![VS2010 템플릿](~/ef6/media/vs2010-templates.png)
