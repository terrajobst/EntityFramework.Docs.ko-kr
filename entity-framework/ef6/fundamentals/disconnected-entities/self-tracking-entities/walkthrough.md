---
title: 자동 추적 엔터티 연습-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 64ca9ae42df1a1c740131e254b8f80f67b2f9f97
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995423"
---
# <a name="self-tracking-entities-walkthrough"></a><span data-ttu-id="00fd8-102">자동 추적 엔터티 연습</span><span class="sxs-lookup"><span data-stu-id="00fd8-102">Self-Tracking Entities Walkthrough</span></span>
> [!IMPORTANT]
> <span data-ttu-id="00fd8-103">자동 추적 엔터티 템플릿을 더 이상 권장하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-103">We no longer recommend using the self-tracking-entities template.</span></span> <span data-ttu-id="00fd8-104">이 템플릿은 기존 응용 프로그램을 지원하는 용도로만 제공될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-104">It will only continue to be available to support existing applications.</span></span> <span data-ttu-id="00fd8-105">응용 프로그램에서 연결이 끊긴 엔터티 그래프를 사용해야 하는 경우 커뮤니티에서 적극적으로 개발한 자동 추적 엔터티와 비슷한 기술인 [추적 가능 엔터티](http://trackableentities.github.io/) 같은 다른 대안을 고려하거나 하위 수준 변경 내용 추적 API를 사용하여 사용자 지정 코드를 작성하는 방법을 고려해 보세요.</span><span class="sxs-lookup"><span data-stu-id="00fd8-105">If your application requires working with disconnected graphs of entities, consider other alternatives such as [Trackable Entities](http://trackableentities.github.io/), which is a technology similar to Self-Tracking-Entities that is more actively developed by the community, or writing custom code using the low-level change tracking APIs.</span></span>

<span data-ttu-id="00fd8-106">이 연습에서는 Windows Communication Foundation (WCF) 서비스 엔터티 그래프를 반환 하는 작업을 노출 하는 시나리오를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-106">This walkthrough demonstrates the scenario in which a Windows Communication Foundation (WCF) service exposes an operation that returns an entity graph.</span></span> <span data-ttu-id="00fd8-107">다음, 클라이언트 응용 프로그램에서는 해당 그래프를 조작 하 고 유효성을 검사 하 고 Entity Framework를 사용 하 여 데이터베이스에 업데이트를 저장 하는 서비스 작업에 수정 내용을 전송 합니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-107">Next, a client application manipulates that graph and submits the modifications to a service operation that validates and saves the updates to a database using Entity Framework.</span></span>

<span data-ttu-id="00fd8-108">이 연습을 완료 하기 전에 읽어야 하는 [자동 추적 엔터티](index.md) 페이지입니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-108">Before completing this walkthrough make sure you read the [Self-Tracking Entities](index.md) page.</span></span>

<span data-ttu-id="00fd8-109">이 연습에서는 다음 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-109">This walkthrough completes the following actions:</span></span>

-   <span data-ttu-id="00fd8-110">에 액세스 하려면 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-110">Creates a database to access.</span></span>
-   <span data-ttu-id="00fd8-111">모델이 포함 된 클래스 라이브러리를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-111">Creates a class library that contains the model.</span></span>
-   <span data-ttu-id="00fd8-112">자동 추적 엔터티 생성기 템플릿을를 교환 합니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-112">Swaps to the Self-Tracking Entity Generator template.</span></span>
-   <span data-ttu-id="00fd8-113">엔터티 클래스를 별도 프로젝트로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-113">Moves the entity classes to a separate project.</span></span>
-   <span data-ttu-id="00fd8-114">쿼리 엔터티를 저장 하는 작업을 노출 하는 WCF 서비스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-114">Creates a WCF service that exposes operations to query and save entities.</span></span>
-   <span data-ttu-id="00fd8-115">응용 프로그램 (콘솔 및 WPF) 서비스를 사용 하는 클라이언트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-115">Creates client applications (Console and WPF) that consume the service.</span></span>

<span data-ttu-id="00fd8-116">이 연습에서는 Database First 사용 하지만 동일한 기술을 Model First를 동일 하 게 적용 합니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-116">We'll use Database First in this walkthrough but the same techniques apply equally to Model First.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="00fd8-117">필수 조건</span><span class="sxs-lookup"><span data-stu-id="00fd8-117">Pre-Requisites</span></span>

<span data-ttu-id="00fd8-118">이 연습을 완료 하려면 최신 버전의 Visual Studio를 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-118">To complete this walkthrough you will need a recent version of Visual Studio.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="00fd8-119">데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="00fd8-119">Create a Database</span></span>

<span data-ttu-id="00fd8-120">Visual Studio와 함께 설치 되는 데이터베이스 서버 설치한 Visual Studio의 버전에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-120">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="00fd8-121">Visual Studio 2012를 사용 하는 경우 다음 만들려는 LocalDB 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-121">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="00fd8-122">Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만드는 됩니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-122">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="00fd8-123">데이터베이스를 생성 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-123">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="00fd8-124">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-124">Open Visual Studio</span></span>
-   <span data-ttu-id="00fd8-125">**보기-&gt; 서버 탐색기**</span><span class="sxs-lookup"><span data-stu-id="00fd8-125">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="00fd8-126">마우스 오른쪽 단추로 클릭 **데이터 연결-&gt; 연결 추가 중...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-126">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="00fd8-127">선택 해야 하기 전에 서버 탐색기에서 데이터베이스에 연결 하지 않았으면 **Microsoft SQL Server** 데이터 원본으로</span><span class="sxs-lookup"><span data-stu-id="00fd8-127">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="00fd8-128">LocalDB 또는 어느에 따라 설치한 SQL Express에 연결</span><span class="sxs-lookup"><span data-stu-id="00fd8-128">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="00fd8-129">입력 **STESample** 데이터베이스 이름으로</span><span class="sxs-lookup"><span data-stu-id="00fd8-129">Enter **STESample** as the database name</span></span>
-   <span data-ttu-id="00fd8-130">선택 **확인** 를 묻는 새 데이터베이스를 만들려는 경우 **예**</span><span class="sxs-lookup"><span data-stu-id="00fd8-130">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="00fd8-131">새 데이터베이스 서버 탐색기에 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-131">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="00fd8-132">Visual Studio 2012를 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="00fd8-132">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="00fd8-133">서버 탐색기에서 데이터베이스를 마우스 오른쪽 단추로 클릭 하 고 선택 **새 쿼리**</span><span class="sxs-lookup"><span data-stu-id="00fd8-133">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="00fd8-134">새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **실행**</span><span class="sxs-lookup"><span data-stu-id="00fd8-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="00fd8-135">Visual Studio 2010을 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="00fd8-135">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="00fd8-136">선택 **데이터&gt; Transact SQL 편집기-&gt; 새 쿼리 연결 하는 중...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-136">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="00fd8-137">입력 **.\\ SQLEXPRESS** 서버 이름 및 클릭으로 **확인**</span><span class="sxs-lookup"><span data-stu-id="00fd8-137">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="00fd8-138">선택 된 **STESample** 쿼리 편집기의 맨 위에 있는 드롭다운에서 아래로 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="00fd8-138">Select the **STESample** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="00fd8-139">새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **SQL 실행**</span><span class="sxs-lookup"><span data-stu-id="00fd8-139">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a><span data-ttu-id="00fd8-140">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="00fd8-140">Create the Model</span></span>

<span data-ttu-id="00fd8-141">Up,에서는 먼저 모델에 삽입할 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-141">First up, we need a project to put the model in.</span></span>

-   <span data-ttu-id="00fd8-142">**파일만&gt; 새로운 기능-&gt; 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-142">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="00fd8-143">선택 **Visual C\#**  왼쪽된 창에서 다음 **클래스 라이브러리**</span><span class="sxs-lookup"><span data-stu-id="00fd8-143">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="00fd8-144">입력 **STESample** 이름과 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="00fd8-144">Enter **STESample** as the name and click **OK**</span></span>

<span data-ttu-id="00fd8-145">이제 간단한 모델을 데이터베이스에 액세스 하려면 EF 디자이너에서 만들어 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-145">Now we'll create a simple model in the EF Designer to access our database:</span></span>

-   <span data-ttu-id="00fd8-146">**프로젝트-&gt; 새 항목 추가...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-146">**Project -&gt; Add New Item...**</span></span>
-   <span data-ttu-id="00fd8-147">선택 **데이터** 왼쪽된 창에서 다음 **ADO.NET 엔터티 데이터 모델**</span><span class="sxs-lookup"><span data-stu-id="00fd8-147">Select **Data** from the left pane and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="00fd8-148">입력 **BloggingModel** 이름과 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="00fd8-148">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="00fd8-149">선택 **데이터베이스에서 생성** 를 클릭 하 고 **다음**</span><span class="sxs-lookup"><span data-stu-id="00fd8-149">Select **Generate from database** and click **Next**</span></span>
-   <span data-ttu-id="00fd8-150">이전 섹션에서 만든 데이터베이스에 대 한 연결 정보를 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-150">Enter the connection information for the database that you created in the previous section</span></span>
-   <span data-ttu-id="00fd8-151">입력 **BloggingContext** 연결 문자열 및 클릭에 대 한 이름으로 **다음**</span><span class="sxs-lookup"><span data-stu-id="00fd8-151">Enter **BloggingContext** as the name for the connection string and click **Next**</span></span>
-   <span data-ttu-id="00fd8-152">옆에 확인란 **테이블** 를 클릭 하 고 **마침**</span><span class="sxs-lookup"><span data-stu-id="00fd8-152">Check the box next to **Tables** and click **Finish**</span></span>

## <a name="swap-to-ste-code-generation"></a><span data-ttu-id="00fd8-153">STE 코드 생성을 교환 합니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-153">Swap to STE Code Generation</span></span>

<span data-ttu-id="00fd8-154">이제 기본 코드 생성 및 교환 작업을 자동 추적 엔터티를 사용 하지 않도록 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-154">Now we need to disable the default code generation and swap to Self-Tracking Entities.</span></span>

### <a name="if-you-are-using-visual-studio-2012"></a><span data-ttu-id="00fd8-155">Visual Studio 2012를 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="00fd8-155">If you are using Visual Studio 2012</span></span>

-   <span data-ttu-id="00fd8-156">확장 **BloggingModel.edmx** 에 **솔루션 탐색기** 삭제 하 고는 **BloggingModel.tt** 고 **BloggingModel.Context.tt** 
     *기본 코드 생성을 사용 하지 않도록 설정 됩니다*</span><span class="sxs-lookup"><span data-stu-id="00fd8-156">Expand **BloggingModel.edmx** in **Solution Explorer** and delete the **BloggingModel.tt** and **BloggingModel.Context.tt**
*This will disable the default code generation*</span></span>
-   <span data-ttu-id="00fd8-157">EF 디자이너 화면에서 빈 영역을 마우스 오른쪽 단추로 클릭 **코드 생성 항목 추가...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-157">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="00fd8-158">선택 **Online** 검색에 대 한 확인 하 고 왼쪽된 창에서 **STE 생성기**</span><span class="sxs-lookup"><span data-stu-id="00fd8-158">Select **Online** from the left pane and search for **STE Generator**</span></span>
-   <span data-ttu-id="00fd8-159">선택 된 **C에 대 한 STE 생성기\#**  템플릿에 입력 **STETemplate** 이름과 클릭 **추가**</span><span class="sxs-lookup"><span data-stu-id="00fd8-159">Select the **STE Generator for C\#** template, enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="00fd8-160">합니다 **STETemplate.tt** 하 고 **STETemplate.Context.tt** 파일은 BloggingModel.edmx 파일 아래에 중첩 추가</span><span class="sxs-lookup"><span data-stu-id="00fd8-160">The **STETemplate.tt** and **STETemplate.Context.tt** files are added nested under the BloggingModel.edmx file</span></span>

### <a name="if-you-are-using-visual-studio-2010"></a><span data-ttu-id="00fd8-161">Visual Studio 2010을 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="00fd8-161">If you are using Visual Studio 2010</span></span>

-   <span data-ttu-id="00fd8-162">EF 디자이너 화면에서 빈 영역을 마우스 오른쪽 단추로 클릭 **코드 생성 항목 추가...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-162">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="00fd8-163">선택 **코드** 왼쪽된 창에서 다음 **ADO.NET 자동 추적 엔터티 생성기**</span><span class="sxs-lookup"><span data-stu-id="00fd8-163">Select **Code** from the left pane and then **ADO.NET Self-Tracking Entity Generator**</span></span>
-   <span data-ttu-id="00fd8-164">입력 **STETemplate** 이름과 클릭 **추가**</span><span class="sxs-lookup"><span data-stu-id="00fd8-164">Enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="00fd8-165">합니다 **STETemplate.tt** 하 고 **STETemplate.Context.tt** 파일은 프로젝트에 직접 추가</span><span class="sxs-lookup"><span data-stu-id="00fd8-165">The **STETemplate.tt** and **STETemplate.Context.tt** files are added directly to your project</span></span>

## <a name="move-entity-types-into-separate-project"></a><span data-ttu-id="00fd8-166">엔터티 형식은 별도 프로젝트로 이동</span><span class="sxs-lookup"><span data-stu-id="00fd8-166">Move Entity Types into Separate Project</span></span>

<span data-ttu-id="00fd8-167">자동 추적 엔터티를 사용 하는 클라이언트 응용 프로그램 모델에서 생성 된 엔터티 클래스에 대 한 액세스를 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-167">To use Self-Tracking Entities our client application needs access to the entity classes generated from our model.</span></span> <span data-ttu-id="00fd8-168">클라이언트 응용 프로그램 전체 모델을 노출 하지 않으려는 것 때문에 엔터티 클래스에는 별도 프로젝트로 이동 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-168">Because we don't want to expose the whole model to the client application we're going to move the entity classes into a separate project.</span></span>

<span data-ttu-id="00fd8-169">첫 번째 단계는 기존 프로젝트에서 엔터티 클래스 생성을 중지 하려면:</span><span class="sxs-lookup"><span data-stu-id="00fd8-169">The first step is to stop generating entity classes in the existing project:</span></span>

-   <span data-ttu-id="00fd8-170">마우스 오른쪽 단추로 클릭 **STETemplate.tt** 에 **솔루션 탐색기** 선택한 **속성**</span><span class="sxs-lookup"><span data-stu-id="00fd8-170">Right-click on **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="00fd8-171">에 **속성** 창 지우기 **TextTemplatingFileGenerator** 에서 합니다 **CustomTool** 속성</span><span class="sxs-lookup"><span data-stu-id="00fd8-171">In the **Properties** window clear **TextTemplatingFileGenerator** from the **CustomTool** property</span></span>
-   <span data-ttu-id="00fd8-172">확장 **STETemplate.tt** 에 **솔루션 탐색기** 그 아래에 중첩 된 모든 파일을 삭제 하 고</span><span class="sxs-lookup"><span data-stu-id="00fd8-172">Expand **STETemplate.tt** in **Solution Explorer** and delete all files nested under it</span></span>

<span data-ttu-id="00fd8-173">새 프로젝트를 추가 하 여 엔터티 클래스를 생성 하겠습니다 다음으로,</span><span class="sxs-lookup"><span data-stu-id="00fd8-173">Next, we are going to add a new project and generate the entity classes in it</span></span>

-   <span data-ttu-id="00fd8-174">**파일만&gt; 추가-&gt; 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-174">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="00fd8-175">선택 **Visual C\#**  왼쪽된 창에서 다음 **클래스 라이브러리**</span><span class="sxs-lookup"><span data-stu-id="00fd8-175">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="00fd8-176">입력 **STESample.Entities** 이름과 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="00fd8-176">Enter **STESample.Entities** as the name and click **OK**</span></span>
-   <span data-ttu-id="00fd8-177">**프로젝트-&gt; 기존 항목 추가...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-177">**Project -&gt; Add Existing Item...**</span></span>
-   <span data-ttu-id="00fd8-178">로 이동 합니다 **STESample** 프로젝트 폴더</span><span class="sxs-lookup"><span data-stu-id="00fd8-178">Navigate to the **STESample** project folder</span></span>
-   <span data-ttu-id="00fd8-179">보려는 선택 **모든 파일 (\*.\*)**</span><span class="sxs-lookup"><span data-stu-id="00fd8-179">Select to view **All Files (\*.\*)**</span></span>
-   <span data-ttu-id="00fd8-180">선택 된 **STETemplate.tt** 파일</span><span class="sxs-lookup"><span data-stu-id="00fd8-180">Select the **STETemplate.tt** file</span></span>
-   <span data-ttu-id="00fd8-181">옆에 드롭다운 화살표를 클릭 합니다 **추가** 단추를 선택 **링크로 추가**</span><span class="sxs-lookup"><span data-stu-id="00fd8-181">Click on the drop down arrow next to the **Add** button and select **Add As Link**</span></span>

    ![AddLinkedTemplate](~/ef6/media/addlinkedtemplate.png)

<span data-ttu-id="00fd8-183">또한 엔터티 클래스에는 동일한 컨텍스트에서 네임 스페이스에 생성 되도록 것입니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-183">We're also going to make sure the entity classes get generated in the same namespace as the context.</span></span> <span data-ttu-id="00fd8-184">이 바로 응용 프로그램 전체에서 추가 해야 하는 문을 사용 하 여의 수를 줄입니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-184">This just reduces the number of using statements we need to add throughout our application.</span></span>

-   <span data-ttu-id="00fd8-185">연결 된 마우스 오른쪽 단추로 클릭 **STETemplate.tt** 에 **솔루션 탐색기** 선택한 **속성**</span><span class="sxs-lookup"><span data-stu-id="00fd8-185">Right-click on the linked **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="00fd8-186">에 **속성** 창 집합 **사용자 지정 도구 Namespace** 에 **STESample**</span><span class="sxs-lookup"><span data-stu-id="00fd8-186">In the **Properties** window set **Custom Tool Namespace** to **STESample**</span></span>

<span data-ttu-id="00fd8-187">STE 템플릿에 의해 생성 된 코드에 대 한 참조 해야 합니다. **System.Runtime.Serialization** 컴파일하기 위해.</span><span class="sxs-lookup"><span data-stu-id="00fd8-187">The code generated by the STE template will need a reference to **System.Runtime.Serialization** in order to compile.</span></span> <span data-ttu-id="00fd8-188">이 라이브러리는 WCF에 필요한 **DataContract** 하 고 **DataMember** 직렬화 가능 엔터티 형식에 사용 되는 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-188">This library is needed for the WCF **DataContract** and **DataMember** attributes that are used on the serializable entity types.</span></span>

-   <span data-ttu-id="00fd8-189">마우스 오른쪽 단추로 클릭 합니다 **STESample.Entities** 프로젝트 **솔루션 탐색기** 선택한 **참조 추가...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-189">Right click on the **STESample.Entities** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="00fd8-190">Visual Studio 2012-확인란 옆 **System.Runtime.Serialization** 를 클릭 하 고 **확인**</span><span class="sxs-lookup"><span data-stu-id="00fd8-190">In Visual Studio 2012 - check the box next to **System.Runtime.Serialization** and click **OK**</span></span>
    -   <span data-ttu-id="00fd8-191">Visual Studio 2010-에서 선택 **System.Runtime.Serialization** 를 클릭 하 고 **확인**</span><span class="sxs-lookup"><span data-stu-id="00fd8-191">In Visual Studio 2010 - select **System.Runtime.Serialization** and click **OK**</span></span>

<span data-ttu-id="00fd8-192">마지막으로,이 컨텍스트에서 사용 하 여 프로젝트에 엔터티 형식에 대 한 참조를 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-192">Finally, the project with our context in it will need a reference to the entity types.</span></span>

-   <span data-ttu-id="00fd8-193">마우스 오른쪽 단추로 클릭 합니다 **STESample** 프로젝트 **솔루션 탐색기** 선택한 **참조 추가...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-193">Right click on the **STESample** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="00fd8-194">Visual Studio 2012-에서 선택 **솔루션** 왼쪽된 창에서 확인란 옆에 **STESample.Entities** 를 클릭 하 고 **확인**</span><span class="sxs-lookup"><span data-stu-id="00fd8-194">In Visual Studio 2012 - select **Solution** from the left pane, check the box next to **STESample.Entities** and click **OK**</span></span>
    -   <span data-ttu-id="00fd8-195">Visual Studio 2010-에서 선택 합니다 **프로젝트** 탭을 선택 **STESample.Entities** 를 클릭 하 고 **확인**</span><span class="sxs-lookup"><span data-stu-id="00fd8-195">In Visual Studio 2010 - select the **Projects** tab, select **STESample.Entities** and click **OK**</span></span>

>[!NOTE]
> <span data-ttu-id="00fd8-196">엔터티 형식은 별도 프로젝트를 이동 하는 또 다른 옵션의 기본 위치에서 연결 하는 것이 아니라 템플릿 파일을 옮기는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-196">Another option for moving the entity types to a separate project is to move the template file, rather than linking it from its default location.</span></span> <span data-ttu-id="00fd8-197">이 작업을 수행 하는 경우 업데이트 해야 합니다는 **inputFile** .edmx 파일에 상대 경로 제공 하는 템플릿에 변수 (이 예제는 **... \\BloggingModel.edmx**).</span><span class="sxs-lookup"><span data-stu-id="00fd8-197">If you do this, you will need to update the **inputFile** variable in the template to provide the relative path to the edmx file (in this example that would be **..\\BloggingModel.edmx**).</span></span>

## <a name="create-a-wcf-service"></a><span data-ttu-id="00fd8-198">WCF 서비스 만들기</span><span class="sxs-lookup"><span data-stu-id="00fd8-198">Create a WCF Service</span></span>

<span data-ttu-id="00fd8-199">이제 데이터를 노출 하는 WCF 서비스를 추가 하려면, 프로젝트를 만들어 시작 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-199">Now it's time to add a WCF Service to expose our data, we'll start by creating the project.</span></span>

-   <span data-ttu-id="00fd8-200">**파일만&gt; 추가-&gt; 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-200">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="00fd8-201">선택 **Visual C\#**  왼쪽된 창에서 다음 **WCF 서비스 응용 프로그램**</span><span class="sxs-lookup"><span data-stu-id="00fd8-201">Select **Visual C\#** from the left pane and then **WCF Service Application**</span></span>
-   <span data-ttu-id="00fd8-202">입력 **STESample.Service** 이름과 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="00fd8-202">Enter **STESample.Service** as the name and click **OK**</span></span>
-   <span data-ttu-id="00fd8-203">에 대 한 참조를 추가 합니다 **System.Data.Entity** 어셈블리</span><span class="sxs-lookup"><span data-stu-id="00fd8-203">Add a reference to the **System.Data.Entity** assembly</span></span>
-   <span data-ttu-id="00fd8-204">에 대 한 참조를 추가 합니다 **STESample** 하 고 **STESample.Entities** 프로젝트</span><span class="sxs-lookup"><span data-stu-id="00fd8-204">Add a reference to the **STESample** and **STESample.Entities** projects</span></span>

<span data-ttu-id="00fd8-205">런타임 시 검색 되도록이 프로젝트에 EF 연결 문자열을 복사 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-205">We need to copy the EF connection string to this project so that it is found at runtime.</span></span>

-   <span data-ttu-id="00fd8-206">열기를 **App.Config** 에 대 한 파일을 * * STESample * * 프로젝트 및 복사 합니다 **connectionStrings** 요소</span><span class="sxs-lookup"><span data-stu-id="00fd8-206">Open the **App.Config** file for the **STESample **project and copy the **connectionStrings** element</span></span>
-   <span data-ttu-id="00fd8-207">붙여넣기를 **connectionStrings** 의 자식 요소로 요소를 **구성** 의 요소를 **Web.Config** 파일을 **STESample.Service** 프로젝트</span><span class="sxs-lookup"><span data-stu-id="00fd8-207">Paste the **connectionStrings** element as a child element of the **configuration** element of the **Web.Config** file in the **STESample.Service** project</span></span>

<span data-ttu-id="00fd8-208">이제 실제 서비스를 구현 하는 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-208">Now it's time to implement the actual service.</span></span>

-   <span data-ttu-id="00fd8-209">오픈 **IService1.cs** 내용을 다음 코드로 바꾸고</span><span class="sxs-lookup"><span data-stu-id="00fd8-209">Open **IService1.cs** and replace the contents with the following code</span></span>

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   <span data-ttu-id="00fd8-210">오픈 **Service1.svc** 내용을 다음 코드로 바꾸고</span><span class="sxs-lookup"><span data-stu-id="00fd8-210">Open **Service1.svc** and replace the contents with the following code</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a><span data-ttu-id="00fd8-211">콘솔 응용 프로그램에서 서비스 사용</span><span class="sxs-lookup"><span data-stu-id="00fd8-211">Consume the Service from a Console Application</span></span>

<span data-ttu-id="00fd8-212">서비스를 사용 하는 콘솔 응용 프로그램을 만들어 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-212">Let's create a console application that uses our service.</span></span>

-   <span data-ttu-id="00fd8-213">**파일만&gt; 새로운 기능-&gt; 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-213">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="00fd8-214">선택 **Visual C\#**  왼쪽된 창에서 다음 **콘솔 응용 프로그램**</span><span class="sxs-lookup"><span data-stu-id="00fd8-214">Select **Visual C\#** from the left pane and then **Console Application**</span></span>
-   <span data-ttu-id="00fd8-215">입력 **STESample.ConsoleTest** 이름과 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="00fd8-215">Enter **STESample.ConsoleTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="00fd8-216">에 대 한 참조를 추가 합니다 **STESample.Entities** 프로젝트</span><span class="sxs-lookup"><span data-stu-id="00fd8-216">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="00fd8-217">WCF 서비스에 대 한 서비스 참조 필요</span><span class="sxs-lookup"><span data-stu-id="00fd8-217">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="00fd8-218">마우스 오른쪽 단추로 클릭 합니다 **STESample.ConsoleTest** 프로젝트 **솔루션 탐색기** 선택한 **서비스 참조 추가...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-218">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="00fd8-219">클릭 **검색**</span><span class="sxs-lookup"><span data-stu-id="00fd8-219">Click **Discover**</span></span>
-   <span data-ttu-id="00fd8-220">입력 **BloggingService** 네임 스페이스와 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="00fd8-220">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="00fd8-221">이제 서비스를 사용 하는 일부 코드를 작성할 수 했습니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-221">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="00fd8-222">오픈 **Program.cs** 그 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-222">Open **Program.cs** and replace the contents with the following code.</span></span>

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

<span data-ttu-id="00fd8-223">이제 응용 프로그램을 실행하여 작동하는지 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-223">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="00fd8-224">마우스 오른쪽 단추로 클릭 합니다 **STESample.ConsoleTest** 프로젝트 **솔루션 탐색기** 선택한 **디버그-&gt; 새 인스턴스 시작**</span><span class="sxs-lookup"><span data-stu-id="00fd8-224">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>

<span data-ttu-id="00fd8-225">응용 프로그램을 실행 하는 경우 다음과 같은 출력이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-225">You'll see the following output when the application executes.</span></span>

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a><span data-ttu-id="00fd8-226">WPF 응용 프로그램에서 서비스 사용</span><span class="sxs-lookup"><span data-stu-id="00fd8-226">Consume the Service from a WPF Application</span></span>

<span data-ttu-id="00fd8-227">서비스를 사용 하는 WPF 응용 프로그램을 만들어 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-227">Let's create a WPF application that uses our service.</span></span>

-   <span data-ttu-id="00fd8-228">**파일만&gt; 새로운 기능-&gt; 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-228">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="00fd8-229">선택 **Visual C\#**  왼쪽된 창에서 다음 **WPF 응용 프로그램**</span><span class="sxs-lookup"><span data-stu-id="00fd8-229">Select **Visual C\#** from the left pane and then **WPF Application**</span></span>
-   <span data-ttu-id="00fd8-230">입력 **STESample.WPFTest** 이름과 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="00fd8-230">Enter **STESample.WPFTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="00fd8-231">에 대 한 참조를 추가 합니다 **STESample.Entities** 프로젝트</span><span class="sxs-lookup"><span data-stu-id="00fd8-231">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="00fd8-232">WCF 서비스에 대 한 서비스 참조 필요</span><span class="sxs-lookup"><span data-stu-id="00fd8-232">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="00fd8-233">마우스 오른쪽 단추로 클릭 합니다 **STESample.WPFTest** 프로젝트 **솔루션 탐색기** 선택한 **서비스 참조 추가...**</span><span class="sxs-lookup"><span data-stu-id="00fd8-233">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="00fd8-234">클릭 **검색**</span><span class="sxs-lookup"><span data-stu-id="00fd8-234">Click **Discover**</span></span>
-   <span data-ttu-id="00fd8-235">입력 **BloggingService** 네임 스페이스와 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="00fd8-235">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="00fd8-236">이제 서비스를 사용 하는 일부 코드를 작성할 수 했습니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-236">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="00fd8-237">오픈 **MainWindow.xaml** 그 내용을 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-237">Open **MainWindow.xaml** and replace the contents with the following code.</span></span>

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   <span data-ttu-id="00fd8-238">MainWindow에 대 한 코드 숨김 엽니다 (**MainWindow.xaml.cs**) 콘텐츠를 다음 코드로 바꾸고</span><span class="sxs-lookup"><span data-stu-id="00fd8-238">Open the code behind for MainWindow (**MainWindow.xaml.cs**) and replace the contents with the following code</span></span>

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

<span data-ttu-id="00fd8-239">이제 응용 프로그램을 실행하여 작동하는지 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="00fd8-239">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="00fd8-240">마우스 오른쪽 단추로 클릭 합니다 **STESample.WPFTest** 프로젝트 **솔루션 탐색기** 선택한 **디버그-&gt; 새 인스턴스 시작**</span><span class="sxs-lookup"><span data-stu-id="00fd8-240">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>
-   <span data-ttu-id="00fd8-241">화면을 사용 하 여 데이터를 조작 하 고 사용 하 여 서비스를 통해 저장 된 **저장할** 단추</span><span class="sxs-lookup"><span data-stu-id="00fd8-241">You can manipulate the data using the screen and save it via the service using the **Save** button</span></span>

![WPF](~/ef6/media/wpf.png)
