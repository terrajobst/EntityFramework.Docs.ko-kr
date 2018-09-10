---
title: 디자이너 엔터티에 분할 EF6
author: divega
ms.date: 2016-10-23
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: 06199be977276cd3656e2550df79bac24276ec51
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250600"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="7808a-102">디자이너 엔터티 분</span><span class="sxs-lookup"><span data-stu-id="7808a-102">Designer Entity Splitting</span></span>
<span data-ttu-id="7808a-103">이 연습에서는 Entity Framework Designer (EF 디자이너)를 사용 하 여 모델을 수정 하 여 엔터티 형식을 두 테이블에 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="7808a-104">테이블이 공통 키를 공유하는 경우 한 엔터티를 여러 테이블에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="7808a-105">두 개의 테이블에 한 엔터티 형식 매핑에 적용되는 개념은 셋 이상의 테이블에 한 엔터티 형식 매핑으로 쉽게 확장됩니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="7808a-106">다음 이미지에서는 EF 디자이너를 사용 하 여 작업할 때 사용 되는 기본 windows를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF 디자이너](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="7808a-108">전제 조건</span><span class="sxs-lookup"><span data-stu-id="7808a-108">Prerequisites</span></span>

<span data-ttu-id="7808a-109">Visual Studio 2012 또는 Visual Studio 2010, Ultimate, Premium, Professional 또는 Web Express 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="7808a-110">데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="7808a-110">Create the Database</span></span>

<span data-ttu-id="7808a-111">Visual Studio와 함께 설치 되는 데이터베이스 서버 설치한 Visual Studio의 버전에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="7808a-112">Visual Studio 2012를 사용 하는 경우 다음 만들려는 LocalDB 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="7808a-113">Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만드는 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="7808a-114">먼저 단일 엔터티로 결합 하겠습니다 두 테이블을 사용 하 여 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="7808a-115">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-115">Open Visual Studio</span></span>
-   <span data-ttu-id="7808a-116">**보기-&gt; 서버 탐색기**</span><span class="sxs-lookup"><span data-stu-id="7808a-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="7808a-117">마우스 오른쪽 단추로 클릭 **데이터 연결-&gt; 연결 추가 중...**</span><span class="sxs-lookup"><span data-stu-id="7808a-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="7808a-118">선택 해야 하기 전에 서버 탐색기에서 데이터베이스에 연결 하지 않았으면 **Microsoft SQL Server** 데이터 원본으로</span><span class="sxs-lookup"><span data-stu-id="7808a-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="7808a-119">LocalDB 또는 어느에 따라 설치한 SQL Express에 연결</span><span class="sxs-lookup"><span data-stu-id="7808a-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="7808a-120">입력 **EntitySplitting** 데이터베이스 이름으로</span><span class="sxs-lookup"><span data-stu-id="7808a-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="7808a-121">선택 **확인** 를 묻는 새 데이터베이스를 만들려는 경우 **예**</span><span class="sxs-lookup"><span data-stu-id="7808a-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="7808a-122">새 데이터베이스 서버 탐색기에 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="7808a-123">Visual Studio 2012를 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="7808a-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="7808a-124">서버 탐색기에서 데이터베이스를 마우스 오른쪽 단추로 클릭 하 고 선택 **새 쿼리**</span><span class="sxs-lookup"><span data-stu-id="7808a-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="7808a-125">새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **실행**</span><span class="sxs-lookup"><span data-stu-id="7808a-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="7808a-126">Visual Studio 2010을 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="7808a-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="7808a-127">선택 **데이터&gt; Transact SQL 편집기-&gt; 새 쿼리 연결 하는 중...**</span><span class="sxs-lookup"><span data-stu-id="7808a-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="7808a-128">입력 **.\\ SQLEXPRESS** 서버 이름 및 클릭으로 **확인**</span><span class="sxs-lookup"><span data-stu-id="7808a-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="7808a-129">선택 된 **EntitySplitting** 쿼리 편집기의 맨 위에 있는 드롭다운에서 아래로 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="7808a-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="7808a-130">새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **SQL 실행**</span><span class="sxs-lookup"><span data-stu-id="7808a-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a><span data-ttu-id="7808a-131">프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="7808a-131">Create the Project</span></span>

-   <span data-ttu-id="7808a-132">**파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="7808a-133">왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔 응용 프로그램** 템플릿.</span><span class="sxs-lookup"><span data-stu-id="7808a-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="7808a-134">입력 **MapEntityToTablesSample** 고 프로젝트의 이름으로 **확인**합니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="7808a-135">클릭 **No** 첫 번째 섹션에서 만든 SQL 쿼리를 저장 하 라는 메시지가 표시 되는 경우.</span><span class="sxs-lookup"><span data-stu-id="7808a-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="7808a-136">데이터베이스를 기반으로 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="7808a-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="7808a-137">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 **추가**를 클릭 하 고 **새 항목**합니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="7808a-138">선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.</span><span class="sxs-lookup"><span data-stu-id="7808a-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="7808a-139">입력 **MapEntityToTablesModel.edmx** 파일 이름 및 클릭 **추가**합니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="7808a-140">Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음입니다.**</span><span class="sxs-lookup"><span data-stu-id="7808a-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="7808a-141">선택 된 **EntitySplitting** 드롭다운에서 연결 아래로 및 클릭 **다음**합니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="7808a-142">데이터베이스 개체 선택 대화 상자에서 확인란 옆에 **테이블** 노드.</span><span class="sxs-lookup"><span data-stu-id="7808a-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="7808a-143">이렇게 하면 모든 테이블에 추가 됩니다는 **EntitySplitting** 모델 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="7808a-144">**마침**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-144">Click **Finish**.</span></span>

<span data-ttu-id="7808a-145">모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="7808a-146">두 테이블에 엔터티 매핑</span><span class="sxs-lookup"><span data-stu-id="7808a-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="7808a-147">이 단계에서는 업데이트를 **Person** 엔터티 형식에서 데이터를 결합할 수는 **Person** 및 **PersonInfo** 테이블.</span><span class="sxs-lookup"><span data-stu-id="7808a-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="7808a-148">선택는 **전자 메일** 및 **Phone** 의 속성을 \* \* PersonInfo \* \* 엔터티 및 키를 누릅니다 **Ctrl + X를 누릅니다** 키.</span><span class="sxs-lookup"><span data-stu-id="7808a-148">Select the **Email** and **Phone** properties of the \*\*PersonInfo \*\*entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="7808a-149">선택 된 \* \* 사용자 \* \* 엔터티 및 키를 눌러 **Ctrl + V** 키입니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-149">Select the \*\*Person \*\*entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="7808a-150">디자인 화면에서 선택 합니다 **PersonInfo** 엔터티와 키를 눌러 **삭제** 키보드에서 단추입니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="7808a-151">클릭 **없음** 제거 하려는 경우 메시지가 표시 되 면를 **PersonInfo** 테이블 모델에서 여기에 매핑할 합니다 **Person** 엔터티.</span><span class="sxs-lookup"><span data-stu-id="7808a-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![테이블 삭제](~/ef6/media/deletetables.png)

<span data-ttu-id="7808a-153">다음 단계는 필요 합니다 **매핑 정보** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="7808a-154">이 창에 표시 되지 않으면 디자인 화면을 마우스 오른쪽 단추로 **매핑 정보**합니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="7808a-155">선택는 **Person** 엔터티 형식 및 클릭 **&lt;테이블이 나 뷰를 추가&gt;** 에 **매핑 정보** 창.</span><span class="sxs-lookup"><span data-stu-id="7808a-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="7808a-156">선택 \* \* PersonInfo \* \* 드롭다운 목록에서.</span><span class="sxs-lookup"><span data-stu-id="7808a-156">Select \*\*PersonInfo \*\* from the drop-down list.</span></span>
    <span data-ttu-id="7808a-157">합니다 **매핑 정보** 창이 기본 열 매핑으로 업데이트 됩니다, 시나리오에 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="7808a-158">**Person** 엔터티 형식이 매핑되는 합니다 **Person** 및 **PersonInfo** 테이블.</span><span class="sxs-lookup"><span data-stu-id="7808a-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![2 매핑](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="7808a-160">모델 사용</span><span class="sxs-lookup"><span data-stu-id="7808a-160">Use the Model</span></span>

-   <span data-ttu-id="7808a-161">Main 메서드에 다음 코드를 붙여 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-161">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   <span data-ttu-id="7808a-162">응용 프로그램을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-162">Compile and run the application.</span></span>

<span data-ttu-id="7808a-163">다음 T-SQL 문은이 응용 프로그램을 실행 한 결과로 데이터베이스에 대해 실행 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="7808a-164">다음 두 **삽입** 문이 컨텍스트를 실행 한 결과로 실행 되었습니다. Savechanges ()입니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="7808a-165">데이터를 차지 합니다 **사용자** 엔터티 간의 분할 및는 **사용자** 및 **PersonInfo** 테이블.</span><span class="sxs-lookup"><span data-stu-id="7808a-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![1 삽입](~/ef6/media/insert1.png)

    ![2 삽입](~/ef6/media/insert2.png)
-   <span data-ttu-id="7808a-168">다음 **선택** 데이터베이스에 사용자를 열거 하는 결과로 실행 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="7808a-169">데이터를 결합 하는 **Person** 및 **PersonInfo** 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="7808a-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![선택](~/ef6/media/select.png)
