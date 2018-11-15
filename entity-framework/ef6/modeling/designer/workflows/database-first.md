---
title: EF6 먼저 데이터베이스
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: c81025fe7c3ad6398f003f7be2a3f9f072eec327
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284085"
---
# <a name="database-first"></a><span data-ttu-id="1283c-102">먼저 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="1283c-102">Database First</span></span>
<span data-ttu-id="1283c-103">이 비디오 및 단계별 연습에서는 Entity Framework를 사용 하 여 Database First 개발에 대 한 소개를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="1283c-104">데이터베이스 먼저를 통해 리버스 엔지니어링 하려면 기존 데이터베이스에서 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="1283c-105">모델은 EDMX 파일 (.edmx 확장명)에 저장 됩니다 및 보고 Entity Framework 디자이너에서 편집할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="1283c-106">응용 프로그램에서 상호 작용 하는 클래스는 EDMX 파일에서 자동으로 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="1283c-107">비디오를 시청 하세요.</span><span class="sxs-lookup"><span data-stu-id="1283c-107">Watch the video</span></span>
<span data-ttu-id="1283c-108">이 비디오에서는 Entity Framework를 사용 하 여 Database First 개발에 대해 소개 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="1283c-109">데이터베이스 먼저를 통해 리버스 엔지니어링 하려면 기존 데이터베이스에서 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="1283c-110">모델은 EDMX 파일 (.edmx 확장명)에 저장 됩니다 및 보고 Entity Framework 디자이너에서 편집할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="1283c-111">응용 프로그램에서 상호 작용 하는 클래스는 EDMX 파일에서 자동으로 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="1283c-112">**작성자**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="1283c-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="1283c-113">**비디오**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="1283c-113">**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="1283c-114">필수 조건</span><span class="sxs-lookup"><span data-stu-id="1283c-114">Pre-Requisites</span></span>

<span data-ttu-id="1283c-115">적어도 Visual studio 2010 해야 하거나이 연습을 완료 하려면 Visual Studio 2012를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="1283c-116">Visual Studio 2010을 사용 하는 경우 해야 할 [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="1283c-117">1. 기존 데이터베이스를 만들려면</span><span class="sxs-lookup"><span data-stu-id="1283c-117">1. Create an Existing Database</span></span>

<span data-ttu-id="1283c-118">일반적으로 이미 만들어집니다, 기존 데이터베이스를 대상으로 할 때 있지만이 연습에 액세스 하려면 데이터베이스를 만들려고 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="1283c-119">Visual Studio와 함께 설치 되는 데이터베이스 서버 설치한 Visual Studio의 버전에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="1283c-120">Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만드는 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="1283c-121">Visual Studio 2012를 사용 하는 경우를 만들 수는 [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="1283c-122">데이터베이스를 생성 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="1283c-123">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-123">Open Visual Studio</span></span>
-   <span data-ttu-id="1283c-124">**보기-&gt; 서버 탐색기**</span><span class="sxs-lookup"><span data-stu-id="1283c-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="1283c-125">마우스 오른쪽 단추로 클릭 **데이터 연결-&gt; 연결 추가 중...**</span><span class="sxs-lookup"><span data-stu-id="1283c-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="1283c-126">Microsoft SQL Server 데이터 원본으로 선택 해야 하기 전에 서버 탐색기에서 데이터베이스에 연결 하지 않은 경우</span><span class="sxs-lookup"><span data-stu-id="1283c-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![데이터 원본 선택](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="1283c-128">LocalDB 또는 어느에 따라 설치한 SQL Express에 연결 하 고 입력 **DatabaseFirst.Blogging** 데이터베이스 이름으로</span><span class="sxs-lookup"><span data-stu-id="1283c-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![Sql Express 연결 DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDB 연결 DF](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="1283c-131">선택 **확인** 를 묻는 새 데이터베이스를 만들려는 경우 **예**</span><span class="sxs-lookup"><span data-stu-id="1283c-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![데이터베이스 대화 상자 만들기](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="1283c-133">새 데이터베이스 이제 서버 탐색기에서 마우스 나타나고 선택 **새 쿼리**</span><span class="sxs-lookup"><span data-stu-id="1283c-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="1283c-134">새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **실행**</span><span class="sxs-lookup"><span data-stu-id="1283c-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);
```

## <a name="2-create-the-application"></a><span data-ttu-id="1283c-135">2. 응용 프로그램 만들기</span><span class="sxs-lookup"><span data-stu-id="1283c-135">2. Create the Application</span></span>

<span data-ttu-id="1283c-136">간단 하 게 데이터 액세스를 수행 하는 데 Database First를 사용 하는 기본적인 콘솔 응용 프로그램을 빌드 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="1283c-137">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-137">Open Visual Studio</span></span>
-   <span data-ttu-id="1283c-138">**파일만&gt; 새로운 기능-&gt; 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="1283c-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="1283c-139">선택 **Windows** 왼쪽된 메뉴에서 및 **콘솔 응용 프로그램**</span><span class="sxs-lookup"><span data-stu-id="1283c-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="1283c-140">입력 **DatabaseFirstSample** 이름으로</span><span class="sxs-lookup"><span data-stu-id="1283c-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="1283c-141">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="1283c-142">3. 모델 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="1283c-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="1283c-143">모델을 만들려면 Visual Studio의 일부로 포함 된 Entity Framework Designer를 사용 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="1283c-144">**프로젝트-&gt; 새 항목 추가...**</span><span class="sxs-lookup"><span data-stu-id="1283c-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="1283c-145">선택 **데이터** 왼쪽된 메뉴에서 차례로 **ADO.NET 엔터티 데이터 모델**</span><span class="sxs-lookup"><span data-stu-id="1283c-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="1283c-146">입력 **BloggingModel** 이름과 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="1283c-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="1283c-147">그러면는 **엔터티 데이터 모델 마법사**</span><span class="sxs-lookup"><span data-stu-id="1283c-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="1283c-148">선택 **데이터베이스에서 생성** 를 클릭 하 고 **다음**</span><span class="sxs-lookup"><span data-stu-id="1283c-148">Select **Generate from Database** and click **Next**</span></span>

    ![마법사 단계 1](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="1283c-150">첫 번째 섹션에서 만든 데이터베이스에 연결을 선택, 입력 **BloggingContext** 연결 문자열 및 클릭의 이름으로 **다음**</span><span class="sxs-lookup"><span data-stu-id="1283c-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![마법사 단계 2](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="1283c-152">모든 테이블을 가져오고 '마침' 클릭 '테이블' 옆의 확인란을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![마법사 단계 3](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="1283c-154">리버스 엔지니어링 프로세스가 완료 되 면 새 모델 프로젝트에 추가 되 고 Entity Framework 디자이너에서 확인할 수 있게 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="1283c-155">또한 App.config 파일을 데이터베이스에 대 한 연결 세부 정보를 사용 하 여 프로젝트에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![초기 모델](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="1283c-157">Visual Studio 2010의 추가 단계</span><span class="sxs-lookup"><span data-stu-id="1283c-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="1283c-158">Visual Studio 2010에서 작업 하는 경우 Entity Framework의 최신 버전으로 업그레이드 하기 위해 수행 해야 할 몇 가지 추가 단계가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="1283c-159">최신 버그 픽스 뿐만 아니라 훨씬 쉽게 사용할 수 있는 향상 된 API 화면 액세스를 제공 하기 때문에 업그레이드 하는 것이 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="1283c-160">먼저를 해야 NuGet에서 최신 버전의 Entity Framework를 가져오려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="1283c-161">\*\*프로젝트&gt; NuGet 패키지 관리... \*\* 
     \*없다면는 \**NuGet 패키지 관리... \*\* 를 설치 해야 하는 옵션을 [최신 버전의 NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="1283c-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="1283c-162">선택 된 **Online** 탭</span><span class="sxs-lookup"><span data-stu-id="1283c-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="1283c-163">선택 된 **EntityFramework** 패키지</span><span class="sxs-lookup"><span data-stu-id="1283c-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="1283c-164">클릭 **설치**</span><span class="sxs-lookup"><span data-stu-id="1283c-164">Click **Install**</span></span>

<span data-ttu-id="1283c-165">다음으로, Entity Framework의 이후 버전에서 도입 된 DbContext api를 사용 하는 코드를 생성할 모델 교환 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="1283c-166">EF 디자이너에서 모델의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 선택 **코드 생성 항목 추가...**</span><span class="sxs-lookup"><span data-stu-id="1283c-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="1283c-167">선택 **온라인 템플릿을** 검색에 대 한 확인 하 고 왼쪽된 메뉴에서 **DbContext**</span><span class="sxs-lookup"><span data-stu-id="1283c-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="1283c-168">선택 된 EF **5.x C에 대 한 DbContext 생성기\#** 를 입력 **BloggingModel** 이름과 클릭 **추가**</span><span class="sxs-lookup"><span data-stu-id="1283c-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContext 템플릿](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="1283c-170">4. 읽기 및 데이터 쓰기</span><span class="sxs-lookup"><span data-stu-id="1283c-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="1283c-171">이제 모델이 준비 일부 데이터 액세스를 사용 하는 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="1283c-172">클래스를 예정 하는 데 데이터에 액세스 되 고 자동으로 생성 된 EDMX 파일에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="1283c-173">*이 스크린 샷에서 Visual Studio 2012, Visual Studio 2010을 사용 하는 경우는 BloggingModel.tt 이며 BloggingModel.Context.tt 파일 프로젝트 아래에 직접 EDMX 파일 아래에 중첩 하지 않고입니다.*</span><span class="sxs-lookup"><span data-stu-id="1283c-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![생성 된 클래스 DF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="1283c-175">아래 표시 된 것과 같이 Program.cs의 Main 메서드를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="1283c-176">이 코드는이 컨텍스트의 새 인스턴스를 만듭니다 및 다음 새 블로그 삽입을 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="1283c-177">다음 LINQ 쿼리를 사용 하 여 제목으로 사전순으로 정렬 하는 데이터베이스에서 모든 블로그를 검색 하려면.</span><span class="sxs-lookup"><span data-stu-id="1283c-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="1283c-178">이제 응용 프로그램을 실행 하 고 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-178">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="1283c-179">5. 데이터베이스 변경 내용 처리</span><span class="sxs-lookup"><span data-stu-id="1283c-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="1283c-180">이제 변경 내용을 반영 하도록 모델을 업데이트 해야 하는 이러한 변경을 수행 하는 경우 데이터베이스 스키마를 일부 변경 하려면 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="1283c-181">첫 번째 단계는 데이터베이스 스키마를 일부 변경 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="1283c-182">스키마에 사용자가 테이블을 추가 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="1283c-183">마우스 오른쪽 단추로 클릭 합니다 **DatabaseFirst.Blogging** 선택한 서버 탐색기에서 데이터베이스 **새 쿼리**</span><span class="sxs-lookup"><span data-stu-id="1283c-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="1283c-184">새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **실행**</span><span class="sxs-lookup"><span data-stu-id="1283c-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="1283c-185">이제 스키마 업데이트 되 면 해당 변경 내용을 사용 하 여 모델을 업데이트 하는 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="1283c-186">그러면 업데이트 마법사가 시작 됩니다, 그리고 EF 디자이너에서 모델 및 선택 ' 데이터베이스에서 모델 업데이트...'의 빈 영역을 마우스 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="1283c-187">추가 탭 업데이트 마법사 확인 테이블 옆의 상자에서 스키마에서 새 테이블을 추가 하려고 한다는 것을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="1283c-188">*새로 고침 탭을 업데이트 하는 동안 변경 내용에 대 한 확인 되는 모델의 모든 기존 테이블을 보여 줍니다. 탭 삭제 스키마에서 제거 된 업데이트의 일부로 모델에서도 제거 됩니다 하는 모든 테이블을 표시 합니다. 이 두 탭의 정보를 자동으로 검색 및 정보 제공의 목적 으로만 제공 됩니다 모든 설정을 변경할 수 없습니다.*</span><span class="sxs-lookup"><span data-stu-id="1283c-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![마법사를 새로 고칩니다.](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="1283c-190">업데이트 마법사에서 마침을 클릭합니다</span><span class="sxs-lookup"><span data-stu-id="1283c-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="1283c-191">모델은 데이터베이스에 추가한 사용자 테이블에 매핑되는 새 사용자 엔터티를 포함 하도록 업데이트 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![모델 업데이트](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="1283c-193">요약</span><span class="sxs-lookup"><span data-stu-id="1283c-193">Summary</span></span>

<span data-ttu-id="1283c-194">이 연습의 첫 번째 데이터베이스 개발에 살펴보았습니다는 덕분에 기존 데이터베이스를 기반으로 EF 디자이너에서 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="1283c-195">그런 다음 해당 모델 읽기 및 쓰기 데이터베이스에서 일부 데이터를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="1283c-196">마지막으로 변경한 데이터베이스 스키마를 반영 하도록 모델을 업데이트 했습니다.</span><span class="sxs-lookup"><span data-stu-id="1283c-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
