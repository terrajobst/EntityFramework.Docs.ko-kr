---
title: Designer CUD 저장 프로시저-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813556"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="5266b-102">Designer CUD 저장 프로시저</span><span class="sxs-lookup"><span data-stu-id="5266b-102">Designer CUD Stored Procedures</span></span>

<span data-ttu-id="5266b-103">이 단계별 연습에서는 Entity Framework Designer (EF Designer)를 사용 하 여 엔터티 형식의 create\\insert, update 및 delete (CUD) 작업을 저장 프로시저에 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="5266b-104"> 기본적으로 Entity Framework는 CUD 작업에 대 한 SQL 문을 자동으로 생성 하지만 저장 프로시저를 이러한 작업에 매핑할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-104"> By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="5266b-105">Code First은 저장 프로시저나 함수에 대 한 매핑을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="5266b-106">그러나 SqlQuery 메서드를 사용 하 여 저장 프로시저 또는 함수를 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="5266b-107">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-107">For example:</span></span>

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="5266b-108">CUD 작업을 저장 프로시저에 매핑할 때의 고려 사항</span><span class="sxs-lookup"><span data-stu-id="5266b-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="5266b-109">CUD 작업을 저장 프로시저에 매핑할 때는 다음과 같은 사항을 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span>

- <span data-ttu-id="5266b-110">CUD 작업 중 하나를 저장 프로시저에 매핑하는 경우 모든 작업을 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="5266b-111">세 가지를 모두 매핑하지 않으면 매핑되지 않은 작업이 실행 되 고  **Updateexception** 이 throw 되는 경우 실패 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
- <span data-ttu-id="5266b-112">저장 프로시저의 모든 매개 변수를 엔터티 속성에 매핑해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-112">You must map every parameter of the stored procedure to entity properties.</span></span>
- <span data-ttu-id="5266b-113">서버에서 삽입 된 행에 대 한 기본 키 값을 생성 하는 경우이 값을 다시 엔터티의 키 속성에 매핑해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="5266b-114">다음 예제에서 **Insertperson** 저장 프로시저는 새로 만든 기본 키를 저장 프로시저의 결과 집합의 일부로 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="5266b-115">기본 키는 EF 디자이너의 기능 **&gt;&lt;결과 바인딩 추가** 를 사용 하 여 엔터티 키 (**PersonID**)에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
- <span data-ttu-id="5266b-116">저장 프로시저 호출은 개념적 모델의 엔터티와 함께 1:1에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="5266b-117">예를 들어 개념적 모델에서 상속 계층 구조를 구현 하 고 **부모** (기본) 및 **자식** (파생 된) 엔터티에 대 한 CUD 저장 프로시저를 매핑하는 경우 **자식** 변경 내용 저장은 **자식의**저장 프로시저만 호출 하 고 **부모의**저장 프로시저 호출을 트리거하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5266b-118">필수 조건</span><span class="sxs-lookup"><span data-stu-id="5266b-118">Prerequisites</span></span>

<span data-ttu-id="5266b-119">이 연습을 완료하려면 다음 사항이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="5266b-120">최신 버전의 Visual Studio입니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="5266b-121">[School 예제 데이터베이스](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="5266b-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="5266b-122">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="5266b-122">Set up the Project</span></span>

- <span data-ttu-id="5266b-123">Visual Studio 2012을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-123">Open Visual Studio 2012.</span></span>
- <span data-ttu-id="5266b-124">**파일-&gt; 새로 만들기-&gt; 프로젝트** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-124">Select **File-&gt; New -&gt; Project**</span></span>
- <span data-ttu-id="5266b-125">왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
- <span data-ttu-id="5266b-126">이름 **으로 을** 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-126">Enter **CUDSProcsSample** as the name.</span></span>
- <span data-ttu-id="5266b-127"> **확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="5266b-128">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="5266b-128">Create a Model</span></span>

- <span data-ttu-id="5266b-129">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가-&gt; 새 항목**을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
- <span data-ttu-id="5266b-130">왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
- <span data-ttu-id="5266b-131">파일 이름 **에 대해 다음** 을 입력 하 고 **추가**를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
- <span data-ttu-id="5266b-132">Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 **다음**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
- <span data-ttu-id="5266b-133"> **새 연결**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-133">Click **New Connection**.</span></span> <span data-ttu-id="5266b-134">연결 속성 대화 상자에서 서버 이름 (\\예: **mssqllocaldb**)을 입력 하 고, 인증 방법을 선택 하 고, 데이터베이스 이름으로 **School** 을 입력 한 다음, **확인**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="5266b-135">데이터 연결 선택 대화 상자가 데이터베이스 연결 설정으로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
- <span data-ttu-id="5266b-136">데이터베이스 개체 선택 대화 상자의 **테이블** 노드에서 **Person** 테이블을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
- <span data-ttu-id="5266b-137">또한 **저장 프로시저 및 함수** 노드에서 **deleteperson**, **insertperson**및 **updateperson**의 저장 프로시저를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span>
- <span data-ttu-id="5266b-138">Visual Studio 2012부터 EF Designer는 저장 프로시저의 대량 가져오기를 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="5266b-139">**선택한 저장 프로시저 및 함수를 엔터티 모델로 가져오기** 는 기본적으로 선택 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="5266b-140">이 예제에서는 엔터티 형식을 삽입, 업데이트 및 삭제 하는 저장 프로시저를 포함 하 고 있으므로이를 가져오지 않고이 확인란의 선택을 취소 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span>

    ![가져오기 S 프로시저](~/ef6/media/importsprocs.jpg)

- <span data-ttu-id="5266b-142"> **마침**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-142">Click **Finish**.</span></span>
    <span data-ttu-id="5266b-143">모델 편집을 위한 디자인 화면을 제공 하는 EF 디자이너가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="5266b-144">Person 엔터티를 저장 프로시저에 매핑</span><span class="sxs-lookup"><span data-stu-id="5266b-144">Map the Person Entity to Stored Procedures</span></span>

- <span data-ttu-id="5266b-145"> **Person** 엔터티 형식을 마우스 오른쪽 단추로 클릭 하 고 **저장 프로시저 매핑**을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
- <span data-ttu-id="5266b-146">저장 프로시저 매핑은 **매핑 정보** 창에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
- <span data-ttu-id="5266b-147"> **&lt;삽입 함수&gt;** 를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="5266b-148">필드가 스토리지 모델의 저장 프로시저를 포함하는 드롭다운 목록이 됩니다. 이 목록의 저장 프로시저를 개념적 모델의 엔터티 형식에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="5266b-149">드롭다운 목록에서 **Insertperson** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-149">Select **InsertPerson** from the drop-down list.</span></span>
- <span data-ttu-id="5266b-150">저장 프로시저 매개 변수와 엔터티 속성 간의 기본 매핑이 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="5266b-151">화살표는 매핑 방향을 나타냅니다. 즉, 속성 값이 저장 프로시저 매개 변수에 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
- <span data-ttu-id="5266b-152"> **&lt;결과 바인딩 추가&gt;** 를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-152">Click **&lt;Add Result Binding&gt;**.</span></span>
- <span data-ttu-id="5266b-153"> **NewPersonID**를 입력 하 고 **insertperson** 저장 프로시저에서 반환 된 매개 변수의 이름을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="5266b-154">선행 또는 후행 공백을 입력 하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-154">Make sure not to type leading or trailing spaces.</span></span>
- <span data-ttu-id="5266b-155"> **Enter**키를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-155">Press **Enter**.</span></span>
- <span data-ttu-id="5266b-156">기본적으로 **NewPersonID** 는 **PersonID**엔터티 키에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="5266b-157">화살표는 매핑 방향을 나타냅니다. 즉, 결과 열의 값이 속성에 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![매핑 정보](~/ef6/media/mappingdetails.png)

- <span data-ttu-id="5266b-159">&lt;클릭 하 여 \*\* 업데이트 함수&gt;\*\*  를 선택 하 고 결과 드롭다운 목록에서 **updateperson** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="5266b-160">저장 프로시저 매개 변수와 엔터티 속성 간의 기본 매핑이 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
- <span data-ttu-id="5266b-161">&lt;클릭 하 여 \*\* 함수&gt; 선택\*\* 하 고 결과 드롭다운 목록에서 **deleteperson** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="5266b-162">저장 프로시저 매개 변수와 엔터티 속성 간의 기본 매핑이 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="5266b-163"> **Person** 엔터티 형식의 삽입, 업데이트 및 삭제 작업이 이제 저장 프로시저에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="5266b-164">저장 프로시저를 사용 하 여 엔터티를 업데이트 하거나 삭제할 때 동시성 검사를 사용 하도록 설정 하려면 다음 옵션 중 하나를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

- <span data-ttu-id="5266b-165">**OUTPUT** 매개 변수를 사용 하 여 저장 프로시저에서 영향을 받는 행 수를 반환 하 고 매개 변수 이름 옆의 **영향을 받는 행 매개 변수** 확인란을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="5266b-166">작업이 호출 될 때 반환 되는 값이 0 이면  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) 이 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
- <span data-ttu-id="5266b-167">동시성 검사에 사용 하려는 속성 옆의 **원래 값 사용** 확인란을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="5266b-168">업데이트를 시도 하면 데이터를 데이터베이스에 다시 쓸 때 원래 데이터베이스에서 읽은 속성의 값이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="5266b-169">값이 데이터베이스의 값과 일치 하지 않으면 **OptimisticConcurrencyException** 이 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="5266b-170">모델 사용</span><span class="sxs-lookup"><span data-stu-id="5266b-170">Use the Model</span></span>

<span data-ttu-id="5266b-171">**Main** 메서드가 정의 된 **Program.cs** 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="5266b-172">Main 함수에 다음 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="5266b-173">이 코드는 새 **Person** 개체를 만든 다음 개체를 업데이트 하 고 마지막으로 개체를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

- <span data-ttu-id="5266b-174">애플리케이션을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-174">Compile and run the application.</span></span> <span data-ttu-id="5266b-175">프로그램은 다음 출력을 생성 합니다. \*</span><span class="sxs-lookup"><span data-stu-id="5266b-175">The program produces the following output \*</span></span>

> [!NOTE]
> <span data-ttu-id="5266b-176">PersonID는 서버에 의해 자동으로 생성 되므로 다른 숫자가 표시 될 가능성이 높습니다. \*</span><span class="sxs-lookup"><span data-stu-id="5266b-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="5266b-177">최종 버전의 Visual Studio에서 작업 하는 경우 Intellitrace를 디버거와 함께 사용 하 여 실행 되는 SQL 문을 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5266b-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![Intellitrace](~/ef6/media/intellitrace.png)
