---
title: 디자이너 엔터티 분할-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415264"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="82825-102">디자이너 엔터티 분할</span><span class="sxs-lookup"><span data-stu-id="82825-102">Designer Entity Splitting</span></span>
<span data-ttu-id="82825-103">이 연습에서는 Entity Framework Designer (EF Designer)를 사용 하 여 모델을 수정 하 여 엔터티 형식을 두 개의 테이블에 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="82825-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="82825-104">테이블이 공통 키를 공유하는 경우 한 엔터티를 여러 테이블에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="82825-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="82825-105">엔터티 형식을 두 개의 테이블에 매핑하는 데 적용 되는 개념은 엔터티 형식을 세 개 이상의 테이블에 매핑하기 쉽도록 쉽게 확장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="82825-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="82825-106">다음 그림에서는 EF Designer를 사용할 때 사용 되는 기본 창을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="82825-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF 디자이너](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="82825-108">필수 조건</span><span class="sxs-lookup"><span data-stu-id="82825-108">Prerequisites</span></span>

<span data-ttu-id="82825-109">Visual Studio 2012 또는 Visual Studio 2010, Ultimate, Premium, Professional 또는 Web Express edition</span><span class="sxs-lookup"><span data-stu-id="82825-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="82825-110">데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="82825-110">Create the Database</span></span>

<span data-ttu-id="82825-111">Visual Studio와 함께 설치 되는 데이터베이스 서버는 설치한 Visual Studio 버전에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="82825-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="82825-112">Visual Studio 2012을 사용 하는 경우 LocalDB 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="82825-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="82825-113">Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="82825-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="82825-114">먼저 단일 엔터티로 결합할 두 개의 테이블이 포함 된 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="82825-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="82825-115">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="82825-115">Open Visual Studio</span></span>
-   <span data-ttu-id="82825-116">**뷰&gt; 서버 탐색기**</span><span class="sxs-lookup"><span data-stu-id="82825-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="82825-117">데이터 연결을 마우스 오른쪽 단추로 클릭 하 **&gt; 연결 추가** ...를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="82825-118">서버 탐색기 데이터베이스에 연결 하지 않은 경우 **Microsoft SQL Server** 를 데이터 원본으로 선택 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="82825-119">설치한 항목에 따라 LocalDB 또는 SQL Express에 연결</span><span class="sxs-lookup"><span data-stu-id="82825-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="82825-120">데이터베이스 이름으로 **EntitySplitting** 을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="82825-121">**확인** 을 선택 하 고 새 데이터베이스를 만들지 여부를 묻는 메시지가 표시 되 면 **예** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="82825-122">이제 새 데이터베이스가 서버 탐색기에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="82825-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="82825-123">Visual Studio 2012을 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="82825-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="82825-124">서버 탐색기에서 데이터베이스를 마우스 오른쪽 단추로 클릭 하 고 **새 쿼리** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="82825-125">다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **실행** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="82825-126">Visual Studio 2010을 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="82825-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="82825-127">**데이터&gt; Transact-sql 편집기-&gt; 새 쿼리 연결** ...을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="82825-128">\\SQLEXPRESS를 서버 이름으로 입력 하 고 **확인을** 클릭 **합니다.**</span><span class="sxs-lookup"><span data-stu-id="82825-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="82825-129">쿼리 편집기 위쪽의 드롭다운에서 **EntitySplitting** 데이터베이스를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="82825-130">다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **Sql 실행** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

## <a name="create-the-project"></a><span data-ttu-id="82825-131">프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="82825-131">Create the Project</span></span>

-   <span data-ttu-id="82825-132">**파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="82825-133">왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔 응용 프로그램** 템플릿을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="82825-134">프로젝트 이름으로 **MapEntityToTablesSample** 를 입력 하 고 **확인을**클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="82825-135">첫 번째 섹션에서 만든 SQL 쿼리를 저장 하 라는 메시지가 표시 되 면 **아니요** 를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="82825-136">데이터베이스를 기반으로 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="82825-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="82825-137">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가**를 가리킨 다음 **새 항목**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="82825-138">왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="82825-139">파일 이름에 대해 **Mapentitytonode.js 모델 .edmx** 를 입력 한 다음 **추가**를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="82825-140">Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 다음을 클릭 **합니다.**</span><span class="sxs-lookup"><span data-stu-id="82825-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="82825-141">드롭다운에서 **EntitySplitting** 연결을 선택 하 고 **다음**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="82825-142">데이터베이스 개체 선택 대화 상자에서 **테이블** 노드 옆의 확인란을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="82825-143">그러면 **EntitySplitting** 데이터베이스의 모든 테이블이 모델에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="82825-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="82825-144"> **마침**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-144">Click **Finish**.</span></span>

<span data-ttu-id="82825-145">모델 편집을 위한 디자인 화면을 제공 하는 Entity Designer 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="82825-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="82825-146">두 테이블에 엔터티 매핑</span><span class="sxs-lookup"><span data-stu-id="82825-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="82825-147">이 단계에서는 person 엔터티 형식이 **person** 및 **PersonInfo** 테이블의 데이터를 결합 하도록 **업데이트 합니다.**</span><span class="sxs-lookup"><span data-stu-id="82825-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="82825-148"> **PersonInfo **엔터티의 **Email** 및 **Phone** 속성을 선택 하 고 **ctrl + X** 키를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="82825-148">Select the **Email** and **Phone** properties of the **PersonInfo **entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="82825-149"> **Person **엔터티를 선택 하 고 **ctrl + V** 키를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="82825-149">Select the **Person **entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="82825-150">디자인 화면에서 **PersonInfo** 엔터티를 선택 하 고 키보드에서 **삭제** 단추를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="82825-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="82825-151">모델에서 **PersonInfo** 테이블을 제거할지 묻는 메시지가 표시 되 면 **아니요** 를 클릭 합니다. 그러면 **Person** 엔터티에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="82825-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![테이블 삭제](~/ef6/media/deletetables.png)

<span data-ttu-id="82825-153">다음 단계를 수행 하려면 **매핑 정보** 창이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="82825-154">이 창이 표시 되지 않으면 디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **매핑 정보**를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="82825-155"> **Person** 엔터티 유형을 선택 하 고 **매핑 정보** 창에서\ \** 테이블 또는 뷰&gt; 추가&lt\;\** 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="82825-156">드롭다운 목록에서 **PersonInfo ** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-156">Select **PersonInfo ** from the drop-down list.</span></span>
    <span data-ttu-id="82825-157"> **매핑 정보** 창은 기본 열 매핑으로 업데이트 되며이는 시나리오에 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="82825-158">이제 **person** 엔터티 형식이 **Person** 및 **PersonInfo** 테이블에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="82825-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![매핑 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="82825-160">모델 사용</span><span class="sxs-lookup"><span data-stu-id="82825-160">Use the Model</span></span>

-   <span data-ttu-id="82825-161">Main 메서드에 다음 코드를 붙여 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="82825-161">Paste the following code in the Main method.</span></span>

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

-   <span data-ttu-id="82825-162">애플리케이션을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-162">Compile and run the application.</span></span>

<span data-ttu-id="82825-163">다음 T-sql 문은이 응용 프로그램을 실행 한 결과로 데이터베이스에 대해 실행 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="82825-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="82825-164">다음 두 **INSERT** 문은 컨텍스트를 실행 한 결과로 실행 되었습니다. SaveChanges ().</span><span class="sxs-lookup"><span data-stu-id="82825-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="82825-165">**Person** 엔터티에서 데이터를 가져와 **person** 테이블과 **PersonInfo** 테이블 간에 분할 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![1 삽입](~/ef6/media/insert1.png)

    ![2 삽입](~/ef6/media/insert2.png)
-   <span data-ttu-id="82825-168">데이터베이스에서 사용자를 열거 한 결과로 다음 **SELECT** 가 실행 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="82825-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="82825-169">**Person** 및 **PersonInfo** 테이블의 데이터를 결합 합니다.</span><span class="sxs-lookup"><span data-stu-id="82825-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![선택](~/ef6/media/select.png)
