---
title: Model First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 1b37805beb3d33f0b6dad2577a8abb3ea8f7b1e4
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182436"
---
# <a name="model-first"></a><span data-ttu-id="164b4-102">Model First</span><span class="sxs-lookup"><span data-stu-id="164b4-102">Model First</span></span>
<span data-ttu-id="164b4-103">이 비디오 및 단계별 연습은 Entity Framework를 사용 하 여 Model First 개발에 대 한 소개를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="164b4-104">Model First를 사용 하면 Entity Framework Designer를 사용 하 여 새 모델을 만든 다음 모델에서 데이터베이스 스키마를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="164b4-105">모델은 EDMX 파일 (.edmx 확장명)에 저장 되며 Entity Framework Designer에서 보고 편집할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="164b4-106">응용 프로그램에서 상호 작용 하는 클래스가 EDMX 파일에서 자동으로 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="164b4-107">비디오 시청</span><span class="sxs-lookup"><span data-stu-id="164b4-107">Watch the video</span></span>
<span data-ttu-id="164b4-108">이 비디오 및 단계별 연습은 Entity Framework를 사용 하 여 Model First 개발에 대 한 소개를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="164b4-109">Model First를 사용 하면 Entity Framework Designer를 사용 하 여 새 모델을 만든 다음 모델에서 데이터베이스 스키마를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="164b4-110">모델은 EDMX 파일 (.edmx 확장명)에 저장 되며 Entity Framework Designer에서 보고 편집할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="164b4-111">응용 프로그램에서 상호 작용 하는 클래스가 EDMX 파일에서 자동으로 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="164b4-112">**제공**: [행](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="164b4-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="164b4-113">**비디오**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv)@NO__T-[1MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="164b4-113">**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="164b4-114">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="164b4-114">Pre-Requisites</span></span>

<span data-ttu-id="164b4-115">이 연습을 완료 하려면 Visual Studio 2010 또는 Visual Studio 2012이 설치 되어 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="164b4-116">Visual Studio 2010을 사용 하는 경우 [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 도 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="164b4-117">1. 애플리케이션 만들기</span><span class="sxs-lookup"><span data-stu-id="164b4-117">1. Create the Application</span></span>

<span data-ttu-id="164b4-118">간단 하 게 유지 하기 위해 Model First를 사용 하 여 데이터 액세스를 수행 하는 기본 콘솔 응용 프로그램을 빌드 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="164b4-119">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-119">Open Visual Studio</span></span>
-   <span data-ttu-id="164b4-120">**파일-&gt; 새 &gt; 프로젝트 ...**</span><span class="sxs-lookup"><span data-stu-id="164b4-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="164b4-121">왼쪽 메뉴 및 **콘솔 응용 프로그램** 에서 **Windows** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="164b4-122">**Modelfirstsample** 을 이름으로 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="164b4-123">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="164b4-124">2. 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="164b4-124">2. Create Model</span></span>

<span data-ttu-id="164b4-125">Visual Studio의 일부로 포함 된 Entity Framework Designer를 사용 하 여 모델을 만들 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="164b4-126">**프로젝트-&gt; 새 항목 추가 ...**</span><span class="sxs-lookup"><span data-stu-id="164b4-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="164b4-127">왼쪽 메뉴에서 **데이터** 를 선택 하 고 **ADO.NET** 를 선택 엔터티 데이터 모델</span><span class="sxs-lookup"><span data-stu-id="164b4-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="164b4-128">이름으로 **BloggingModel** 를 입력 하 고 **확인**을 클릭 하면 엔터티 데이터 모델 마법사가 시작 됩니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="164b4-129">**빈 모델** 을 선택 하 고 **마침** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-129">Select **Empty Model** and click **Finish**</span></span>

    ![빈 모델 만들기](~/ef6/media/createemptymodel.png)

<span data-ttu-id="164b4-131">빈 모델을 사용 하 여 Entity Framework Designer를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="164b4-132">이제 모델에 엔터티, 속성 및 연결을 추가 하기 시작할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="164b4-133">디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **속성** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="164b4-134">속성 창에서 **엔터티 컨테이너 이름을** **BloggingContext**
    로 변경 합니다.*이 이름은 사용자를 위해 생성 되는 파생 컨텍스트의 이름이 며, 컨텍스트는 데이터베이스와의 세션을 나타내며, 쿼리 및 저장을 허용 합니다. 데이터*</span><span class="sxs-lookup"><span data-stu-id="164b4-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="164b4-135">디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **추가 새-&gt; 엔터티** ...를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="164b4-136">**블로그** 를 엔터티 이름으로 입력 하 고 키 이름으로 **BlogId** 를 입력 하 고 **확인을** 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![블로그 엔터티 추가](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="164b4-138">디자인 화면에서 새 엔터티를 마우스 오른쪽 단추로 클릭 하 고 **Add new-&gt; 스칼라 속성**을 선택 하 고 **name** 을 속성 이름으로 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="164b4-139">**Url** 속성을 추가 하려면이 프로세스를 반복 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="164b4-140">디자인 화면에서 **url** 속성을 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 하 속성 창 **Nullable** 설정을 **True**
    로 변경 합니다.\*이를 통해 url을 할당 하지 않고 데이터베이스에 블로그를 저장할 수 있습니다. \*</span><span class="sxs-lookup"><span data-stu-id="164b4-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="164b4-141">방금 학습 된 기법을 사용 하 여 **PostId** key 속성이 있는 **Post** 엔터티를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="164b4-142">**게시** 엔터티에 **제목** 및 **내용** 스칼라 속성 추가</span><span class="sxs-lookup"><span data-stu-id="164b4-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="164b4-143">이제 두 개의 엔터티가 있으므로 두 엔터티 간에 연결 (또는 관계)을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="164b4-144">디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **추가 &gt; 연결 추가** ...를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="164b4-145">복합성이 **하나** 이 고 다른 끝점은 **여러**
    의 다중성 **을 사용 하** 여 **게시물** 에 대 한 관계 지점의 한쪽 끝을 만듭니다 .이는*블로그에 많은 게시물이 있고 게시물이 하나의 블로그에 속해 있음을 의미* 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="164b4-146">**' Post ' 엔터티 상자에 외래 키 속성 추가** 가 선택 되어 있는지 확인 하 고 **확인** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![연결 MF 추가](~/ef6/media/addassociationmf.png)

<span data-ttu-id="164b4-148">이제에서 데이터베이스를 생성 하 고 데이터를 읽고 쓰는 데 사용할 수 있는 간단한 모델을 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![모델 초기](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="164b4-150">Visual Studio 2010의 추가 단계</span><span class="sxs-lookup"><span data-stu-id="164b4-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="164b4-151">Visual Studio 2010에서 작업 하는 경우 Entity Framework 최신 버전으로 업그레이드 하기 위해 몇 가지 추가 단계를 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="164b4-152">업그레이드는 향상 된 API 화면에 대 한 액세스를 제공 하 고, 사용 하기 쉽고, 최신 버그 수정 기능을 제공 하기 때문에 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="164b4-153">먼저 NuGet에서 최신 버전의 Entity Framework을 가져와야 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="164b4-154">**프로젝트 – &gt; NuGet 패키지 관리** ... 
     nuget\* **패키지 관리 ...** 옵션이 없는 경우 [최신 버전의 nuget](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 을 설치 해야\* 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="164b4-155">**온라인** 탭을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="164b4-156">**Entityframework** 패키지를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="164b4-157">**설치** 클릭</span><span class="sxs-lookup"><span data-stu-id="164b4-157">Click **Install**</span></span>

<span data-ttu-id="164b4-158">다음으로 Entity Framework의 이후 버전에 도입 된 DbContext API를 사용 하는 코드를 생성 하기 위해 모델을 교환 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="164b4-159">EF 디자이너에서 모델의 빈 지점을 마우스 오른쪽 단추로 클릭 하 고 **코드 생성 항목 추가** ...를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="164b4-160">왼쪽 메뉴에서 **온라인 템플릿** 을 선택 하 고 **DbContext** 를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="164b4-161">EF **.x DbContext 생성기 for C @ no__t-1**을 선택 하 고 이름으로 **BloggingModel** 를 입력 한 다음 **추가** 를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContext 템플릿](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="164b4-163">3. 데이터베이스 생성</span><span class="sxs-lookup"><span data-stu-id="164b4-163">3. Generating the Database</span></span>

<span data-ttu-id="164b4-164">모델을 통해 모델을 사용 하 여 데이터를 저장 하 고 검색 하는 데 사용할 수 있는 데이터베이스 스키마를 계산할 수 Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="164b4-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="164b4-165">Visual Studio와 함께 설치 되는 데이터베이스 서버는 설치한 Visual Studio 버전에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="164b4-166">Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="164b4-167">Visual Studio 2012을 사용 하는 경우 [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="164b4-168">계속 해 서 데이터베이스를 생성 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="164b4-169">디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **모델에서 데이터베이스 생성** ...을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="164b4-170">**새 연결** ...을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-170">Click **New Connection…**</span></span> <span data-ttu-id="164b4-171">사용 중인 Visual Studio 버전에 따라 LocalDB 또는 SQL Express를 지정 하 고 데이터베이스 이름으로 **Modelfirst** 를 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![LocalDB 연결 MF](~/ef6/media/localdbconnectionmf.png)

    ![Sql Express 연결 MF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="164b4-174">**확인** 을 선택 하 고 새 데이터베이스를 만들지 여부를 묻는 메시지가 표시 되 면 **예** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="164b4-175">**다음** 을 선택 하면 Entity Framework Designer는 데이터베이스 스키마를 만드는 스크립트를 계산 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="164b4-176">스크립트가 표시 되 면 **마침** 을 클릭 하면 스크립트가 프로젝트에 추가 되 고 열립니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="164b4-177">스크립트를 마우스 오른쪽 단추로 클릭 하 고 **실행**을 선택 합니다. 사용 중인 Visual Studio 버전에 따라 연결할 데이터베이스를 지정 하 고 LocalDB 또는 SQL Server Express를 지정 하 라는 메시지가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="164b4-178">4. 데이터 읽기 & 쓰기</span><span class="sxs-lookup"><span data-stu-id="164b4-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="164b4-179">이제 모델을 사용 하 여 일부 데이터에 액세스 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="164b4-180">데이터에 액세스 하는 데 사용할 클래스는 EDMX 파일을 기반으로 자동으로 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="164b4-181">*이 스크린샷은 visual Studio 2012에서 가져온 것입니다. Visual Studio 2010를 사용 하는 경우 BloggingModel.tt 및 BloggingModel.Context.tt 파일은 EDMX 파일 아래에 중첩 되지 않고 직접 프로젝트 바로 아래에 있습니다.*</span><span class="sxs-lookup"><span data-stu-id="164b4-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![생성 된 클래스](~/ef6/media/generatedclasses.png)

<span data-ttu-id="164b4-183">아래와 같이 Program.cs에서 Main 메서드를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="164b4-184">이 코드는 컨텍스트의 새 인스턴스를 만든 다음이를 사용 하 여 새 블로그를 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="164b4-185">그런 다음 LINQ 쿼리를 사용 하 여 데이터베이스에서 제목별로 사전순으로 정렬 된 모든 블로그를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="164b4-186">이제 응용 프로그램을 실행 하 고 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-186">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="164b4-187">5. 모델 변경 내용 처리</span><span class="sxs-lookup"><span data-stu-id="164b4-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="164b4-188">이제 모델을 변경할 때 이러한 변경 작업을 수행 하면 데이터베이스 스키마도 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="164b4-189">모델에 새 사용자 엔터티를 추가 하는 것부터 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="164b4-190">키 이름 및 **문자열** 을 키 이름 **으로 사용 하 여 새** **사용자** 엔터티 이름을 키에 대 한 속성 형식으로 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![사용자 엔터티 추가](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="164b4-192">디자인 화면에서 **username** 속성을 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 하 속성 창 **MaxLength** 설정을 **50**
    로 변경 합니다.*이렇게 하면 사용자 이름에 저장할 수 있는 데이터를 50으로 제한 합니다. 문자*</span><span class="sxs-lookup"><span data-stu-id="164b4-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="164b4-193">**사용자** 엔터티에 **DisplayName** 스칼라 속성 추가</span><span class="sxs-lookup"><span data-stu-id="164b4-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="164b4-194">이제 업데이트 된 모델이 있으며 새 사용자 엔터티 유형을 수용 하기 위해 데이터베이스를 업데이트할 준비가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="164b4-195">디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **모델에서 데이터베이스 생성**...을 선택 하 여 업데이트 된 모델을 기반으로 스키마를 다시 만드는 스크립트를 계산 Entity Framework 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="164b4-196">**마침** 클릭</span><span class="sxs-lookup"><span data-stu-id="164b4-196">Click **Finish**</span></span>
-   <span data-ttu-id="164b4-197">기존 DDL 스크립트와 모델의 매핑 및 저장소 부분을 덮어쓰는 방법에 대 한 경고 메시지가 표시 될 수 있습니다. 이러한 경고에 대해 모두 **예** 를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="164b4-198">데이터베이스를 만들기 위해 업데이트 된 SQL 스크립트가 열립니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="164b4-199">@no__t-생성 된 스크립트는 모든 기존 테이블을 삭제 한 다음 스키마를 처음부터 다시 만듭니다. 로컬 개발에는 적용 되지만 이미 배포 된 데이터베이스에 변경 내용을 푸시할 수는 없습니다. 이미 배포 된 데이터베이스에 변경 내용을 게시 해야 하는 경우에는 스크립트를 편집 하거나 스키마 비교 도구를 사용 하 여 마이그레이션 스크립트를 계산 해야 합니다. \*</span><span class="sxs-lookup"><span data-stu-id="164b4-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="164b4-200">스크립트를 마우스 오른쪽 단추로 클릭 하 고 **실행**을 선택 합니다. 사용 중인 Visual Studio 버전에 따라 연결할 데이터베이스를 지정 하 고 LocalDB 또는 SQL Server Express를 지정 하 라는 메시지가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="164b4-201">요약</span><span class="sxs-lookup"><span data-stu-id="164b4-201">Summary</span></span>

<span data-ttu-id="164b4-202">이 연습에서는 EF Designer에서 모델을 만든 다음 해당 모델에서 데이터베이스를 생성할 수 있도록 하는 Model First 개발에 대해 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="164b4-203">그런 다음 모델을 사용 하 여 데이터베이스에서 일부 데이터를 읽고 씁니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="164b4-204">마지막으로 모델을 업데이트 한 다음 모델과 일치 하도록 데이터베이스 스키마를 다시 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="164b4-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
