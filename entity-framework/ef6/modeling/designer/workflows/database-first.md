---
title: Database First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: d40cff4ddccf43a394ef4f244653372a5a89b05a
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182458"
---
# <a name="database-first"></a><span data-ttu-id="1e431-102">Database First</span><span class="sxs-lookup"><span data-stu-id="1e431-102">Database First</span></span>
<span data-ttu-id="1e431-103">이 비디오 및 단계별 연습은 Entity Framework를 사용 하 여 Database First 개발에 대 한 소개를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="1e431-104">Database First를 사용 하 여 기존 데이터베이스에서 모델을 리버스 엔지니어링할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="1e431-105">모델은 EDMX 파일 (.edmx 확장명)에 저장 되며 Entity Framework Designer에서 보고 편집할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="1e431-106">응용 프로그램에서 상호 작용 하는 클래스가 EDMX 파일에서 자동으로 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="1e431-107">비디오 시청</span><span class="sxs-lookup"><span data-stu-id="1e431-107">Watch the video</span></span>
<span data-ttu-id="1e431-108">이 비디오는 Entity Framework를 사용 하 여 Database First 개발에 대 한 소개를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="1e431-109">Database First를 사용 하 여 기존 데이터베이스에서 모델을 리버스 엔지니어링할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="1e431-110">모델은 EDMX 파일 (.edmx 확장명)에 저장 되며 Entity Framework Designer에서 보고 편집할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="1e431-111">응용 프로그램에서 상호 작용 하는 클래스가 EDMX 파일에서 자동으로 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="1e431-112">**제공**: [행](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="1e431-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="1e431-113">**비디오**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv)@NO__T-[1MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="1e431-113">**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="1e431-114">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="1e431-114">Pre-Requisites</span></span>

<span data-ttu-id="1e431-115">이 연습을 완료 하려면 Visual Studio 2010 또는 Visual Studio 2012 이상이 설치 되어 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="1e431-116">Visual Studio 2010을 사용 하는 경우 [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 도 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="1e431-117">1. 기존 데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="1e431-117">1. Create an Existing Database</span></span>

<span data-ttu-id="1e431-118">일반적으로 기존 데이터베이스를 대상으로 하는 경우에는 이미 생성 되지만이 연습에서는 액세스할 데이터베이스를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="1e431-119">Visual Studio와 함께 설치 되는 데이터베이스 서버는 설치한 Visual Studio 버전에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="1e431-120">Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="1e431-121">Visual Studio 2012을 사용 하는 경우 [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="1e431-122">계속 해 서 데이터베이스를 생성 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="1e431-123">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-123">Open Visual Studio</span></span>
-   <span data-ttu-id="1e431-124">**뷰-&gt; 서버 탐색기**</span><span class="sxs-lookup"><span data-stu-id="1e431-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="1e431-125">데이터 연결을 마우스 오른쪽 단추로 클릭 하 **&gt; 연결 추가** ...를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="1e431-126">서버 탐색기 데이터베이스에 연결 하지 않은 경우 Microsoft SQL Server를 데이터 원본으로 선택 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![데이터 원본 선택](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="1e431-128">설치한 항목에 따라 LocalDB 또는 SQL Express에 연결 하 고 **Databasefirst. 블로그** 를 데이터베이스 이름으로 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![Sql Express 연결 DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDB 연결 DF](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="1e431-131">**확인** 을 선택 하 고 새 데이터베이스를 만들지 여부를 묻는 메시지가 표시 되 면 **예** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![데이터베이스 만들기 대화 상자](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="1e431-133">이제 새 데이터베이스가 서버 탐색기에 표시 되 면 마우스 오른쪽 단추로 클릭 하 고 **새 쿼리** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="1e431-134">다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **실행** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="1e431-135">2. 애플리케이션 만들기</span><span class="sxs-lookup"><span data-stu-id="1e431-135">2. Create the Application</span></span>

<span data-ttu-id="1e431-136">간단 하 게 유지 하기 위해 Database First를 사용 하 여 데이터 액세스를 수행 하는 기본 콘솔 응용 프로그램을 빌드 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="1e431-137">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-137">Open Visual Studio</span></span>
-   <span data-ttu-id="1e431-138">**파일-&gt; 새 &gt; 프로젝트 ...**</span><span class="sxs-lookup"><span data-stu-id="1e431-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="1e431-139">왼쪽 메뉴 및 **콘솔 응용 프로그램** 에서 **Windows** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="1e431-140">**Databasefirstsample** 을 이름으로 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="1e431-141">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="1e431-142">3. 리버스 엔지니어링 모델</span><span class="sxs-lookup"><span data-stu-id="1e431-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="1e431-143">Visual Studio의 일부로 포함 된 Entity Framework Designer를 사용 하 여 모델을 만들 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="1e431-144">**프로젝트-&gt; 새 항목 추가 ...**</span><span class="sxs-lookup"><span data-stu-id="1e431-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="1e431-145">왼쪽 메뉴에서 **데이터** 를 선택 하 고 **ADO.NET** 를 선택 엔터티 데이터 모델</span><span class="sxs-lookup"><span data-stu-id="1e431-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="1e431-146">이름으로 **BloggingModel** 를 입력 하 고 **확인을** 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="1e431-147">그러면 **엔터티 데이터 모델 마법사** 가 시작 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="1e431-148">**데이터베이스에서 생성** 을 선택 하 고 **다음** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-148">Select **Generate from Database** and click **Next**</span></span>

    ![마법사 1 단계](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="1e431-150">첫 번째 섹션에서 만든 데이터베이스에 대 한 연결을 선택 하 고 연결 문자열 이름으로 **BloggingContext** 를 입력 하 고 **다음** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![마법사 2 단계](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="1e431-152">' 테이블 ' 옆의 확인란을 클릭 하 여 모든 테이블을 가져온 다음 ' 마침 '을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![마법사 3 단계](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="1e431-154">리버스 엔지니어링 프로세스가 완료 되 면 새 모델이 프로젝트에 추가 되 고 Entity Framework Designer에서 볼 수 있도록 열립니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="1e431-155">또한 데이터베이스에 대 한 연결 세부 정보를 사용 하 여 App.config 파일이 프로젝트에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![모델 초기](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="1e431-157">Visual Studio 2010의 추가 단계</span><span class="sxs-lookup"><span data-stu-id="1e431-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="1e431-158">Visual Studio 2010에서 작업 하는 경우 Entity Framework 최신 버전으로 업그레이드 하기 위해 몇 가지 추가 단계를 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="1e431-159">업그레이드는 향상 된 API 화면에 대 한 액세스를 제공 하 고, 사용 하기 쉽고, 최신 버그 수정 기능을 제공 하기 때문에 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="1e431-160">먼저 NuGet에서 최신 버전의 Entity Framework을 가져와야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="1e431-161">**프로젝트 – &gt; NuGet 패키지 관리** ... 
     nuget\* **패키지 관리 ...** 옵션이 없는 경우 [최신 버전의 nuget](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 을 설치 해야\* 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="1e431-162">**온라인** 탭을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="1e431-163">**Entityframework** 패키지를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="1e431-164">**설치** 클릭</span><span class="sxs-lookup"><span data-stu-id="1e431-164">Click **Install**</span></span>

<span data-ttu-id="1e431-165">다음으로 Entity Framework의 이후 버전에 도입 된 DbContext API를 사용 하는 코드를 생성 하기 위해 모델을 교환 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="1e431-166">EF 디자이너에서 모델의 빈 지점을 마우스 오른쪽 단추로 클릭 하 고 **코드 생성 항목 추가** ...를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="1e431-167">왼쪽 메뉴에서 **온라인 템플릿** 을 선택 하 고 **DbContext** 를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="1e431-168">EF **.x DbContext 생성기 for C @ no__t-1**을 선택 하 고 이름으로 **BloggingModel** 를 입력 한 다음 **추가** 를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContext 템플릿](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="1e431-170">4. 데이터 읽기 & 쓰기</span><span class="sxs-lookup"><span data-stu-id="1e431-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="1e431-171">이제 모델을 사용 하 여 일부 데이터에 액세스 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="1e431-172">데이터에 액세스 하는 데 사용할 클래스는 EDMX 파일을 기반으로 자동으로 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="1e431-173">*이 스크린샷은 visual Studio 2012에서 가져온 것입니다. Visual Studio 2010를 사용 하는 경우 BloggingModel.tt 및 BloggingModel.Context.tt 파일은 EDMX 파일 아래에 중첩 되지 않고 직접 프로젝트 바로 아래에 있습니다.*</span><span class="sxs-lookup"><span data-stu-id="1e431-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![생성 된 클래스 DF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="1e431-175">아래와 같이 Program.cs에서 Main 메서드를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="1e431-176">이 코드는 컨텍스트의 새 인스턴스를 만든 다음이를 사용 하 여 새 블로그를 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="1e431-177">그런 다음 LINQ 쿼리를 사용 하 여 데이터베이스에서 제목별로 사전순으로 정렬 된 모든 블로그를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="1e431-178">이제 응용 프로그램을 실행 하 고 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-178">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="1e431-179">5. 데이터베이스 변경 내용 처리</span><span class="sxs-lookup"><span data-stu-id="1e431-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="1e431-180">이제 데이터베이스 스키마를 변경할 때 이러한 변경을 수행할 때 이러한 변경 내용을 반영 하기 위해 모델을 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="1e431-181">첫 번째 단계는 데이터베이스 스키마를 변경 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="1e431-182">사용자 테이블을 스키마에 추가 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="1e431-183">Databasefirst를 마우스 오른쪽 단추로 클릭 하 고 서버 탐색기의 블로그 데이터베이스를 클릭 한 다음 **새 쿼리** 를 선택 **합니다.**</span><span class="sxs-lookup"><span data-stu-id="1e431-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="1e431-184">다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **실행** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="1e431-185">이제 스키마가 업데이트 되었으므로 모델을 해당 변경 내용으로 업데이트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="1e431-186">EF 디자이너에서 모델의 빈 지점을 마우스 오른쪽 단추로 클릭 하 고 ' 데이터베이스에서 모델 업데이트 ... '를 선택 하면 업데이트 마법사가 시작 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="1e431-187">업데이트 마법사의 추가 탭에서 테이블 옆의 확인란을 선택 합니다 .이는 스키마에서 새 테이블을 추가 하려고 함을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="1e431-188">@no__t-새로 고침 탭에는 업데이트 중에 변경 내용이 확인 될 모델의 기존 테이블이 표시 됩니다. 삭제 탭에는 스키마에서 제거 되 고 업데이트의 일부로 모델에서 제거 되는 모든 테이블이 표시 됩니다. 이러한 두 탭에 대 한 정보는 자동으로 검색 되며 정보 제공을 위해서만 제공 되며 설정을 변경할 수 없습니다. \*</span><span class="sxs-lookup"><span data-stu-id="1e431-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![새로 고침 마법사](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="1e431-190">업데이트 마법사에서 마침을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="1e431-191">이제 모델은 데이터베이스에 추가한 사용자 테이블에 매핑되는 새 사용자 엔터티를 포함 하도록 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![모델이 업데이트 됨](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="1e431-193">요약</span><span class="sxs-lookup"><span data-stu-id="1e431-193">Summary</span></span>

<span data-ttu-id="1e431-194">이 연습에서는 기존 데이터베이스를 기반으로 EF Designer에서 모델을 만들 수 있도록 하는 Database First 개발을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="1e431-195">그런 다음이 모델을 사용 하 여 데이터베이스에서 일부 데이터를 읽고 씁니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="1e431-196">마지막으로, 데이터베이스 스키마에 대 한 변경 내용을 반영 하도록 모델을 업데이트 했습니다.</span><span class="sxs-lookup"><span data-stu-id="1e431-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
