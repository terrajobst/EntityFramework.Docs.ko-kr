---
title: 저장 프로시저-EF6 디자이너 CUD
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 35a00aa817c8643352c517c233977efd49e3baac
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489559"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="3f512-102">디자이너 CUD 저장 프로시저</span><span class="sxs-lookup"><span data-stu-id="3f512-102">Designer CUD Stored Procedures</span></span>
<span data-ttu-id="3f512-103">이 단계별 연습 만들기를 매핑하는 방법을 보여 줍니다\\삽입, 업데이트 및 삭제 작업 (CUD) Entity Framework Designer (EF 디자이너)를 사용 하 여 저장된 프로시저에 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span>  <span data-ttu-id="3f512-104">기본적으로 Entity Framework에 CUD 작업에 대 한 SQL 문을 자동으로 생성 되지만 이러한 작업에 저장된 프로시저도 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-104">By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="3f512-105">Code First에서 지원 하지 않음을 저장 프로시저나 함수에 대 한 매핑 참고 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="3f512-106">그러나 System.Data.Entity.DbSet.SqlQuery 메서드를 사용 하 여 저장된 프로시저 또는 함수를 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="3f512-107">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="3f512-107">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="3f512-108">저장 프로시저를 CUD 작업을 매핑할 때의 고려 사항</span><span class="sxs-lookup"><span data-stu-id="3f512-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="3f512-109">CUD 작업 저장된 프로시저를 매핑할 경우 다음 사항을 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span> 

-   <span data-ttu-id="3f512-110">CUD 작업 중 하나는 저장된 프로시저에 매핑하는, 하는 경우 모두를 매핑하십시오.</span><span class="sxs-lookup"><span data-stu-id="3f512-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="3f512-111">실행 하는 경우 매핑되지 않은 작업이 모든 3으로 매핑되지 않습니다 못합니다 **UpdateException** throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
-   <span data-ttu-id="3f512-112">엔터티 속성에 저장된 프로시저의 모든 매개 변수에 매핑되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-112">You must map every parameter of the stored procedure to entity properties.</span></span>
-   <span data-ttu-id="3f512-113">삽입 된 행에 대 한 기본 키 값을 생성 하는 서버,이 값이 엔터티의 키 속성에 다시 매핑해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="3f512-114">나오는 예제는 **InsertPerson** 저장된 프로시저는 저장된 프로시저의 결과 집합의 일부로 새로 만든된 기본 키를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="3f512-115">기본 키가 엔터티 키에 매핑된 (**PersonID**)를 사용 하는 **&lt;Binding 추가&gt;** EF 디자이너의 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
-   <span data-ttu-id="3f512-116">저장된 프로시저 호출은 개념적 모델의 엔터티를 사용 하 여 매핑된 1:1입니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="3f512-117">예를 들어, 한 다음 맵 고 개념적 모델의 상속 계층 구조를 구현 하는 경우는 CUD 저장 프로시저에 대 한 합니다 **부모** (기본) 및 **자식** (파생 된) 엔터티를 저장 합니다 **자식** 변경만 호출 됩니다는 **자식**저장 프로시저의 트리거되지 않습니다 합니다 **부모**저장 된 프로시저 호출의 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f512-118">전제 조건</span><span class="sxs-lookup"><span data-stu-id="3f512-118">Prerequisites</span></span>

<span data-ttu-id="3f512-119">이 연습을 완료하려면 다음 사항이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="3f512-120">Visual Studio의 최신 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="3f512-121">합니다 [School 샘플 데이터베이스](~/ef6/resources/school-database.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="3f512-122">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="3f512-122">Set up the Project</span></span>

-   <span data-ttu-id="3f512-123">Visual Studio 2012를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-123">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="3f512-124">선택 **파일-&gt; 새로운 기능-&gt; 프로젝트**</span><span class="sxs-lookup"><span data-stu-id="3f512-124">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="3f512-125">왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿.</span><span class="sxs-lookup"><span data-stu-id="3f512-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="3f512-126">입력 **CUDSProcsSample** 이름으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-126">Enter **CUDSProcsSample** as the name.</span></span>
-   <span data-ttu-id="3f512-127">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="3f512-128">모델 만들기</span><span class="sxs-lookup"><span data-stu-id="3f512-128">Create a Model</span></span>

-   <span data-ttu-id="3f512-129">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 누르고 **추가-&gt; 새 항목**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="3f512-130">선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.</span><span class="sxs-lookup"><span data-stu-id="3f512-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="3f512-131">입력 **CUDSProcs.edmx** 파일 이름 및 클릭 **추가**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="3f512-132">Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="3f512-133">클릭 **새 연결**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-133">Click **New Connection**.</span></span> <span data-ttu-id="3f512-134">연결 속성 대화 상자에서 서버 이름을 입력 합니다 (예를 들어 **(localdb)\\mssqllocaldb**)을 선택 인증 방법, 형식 **학교** 데이터베이스 이름에 대 한 다음 클릭 **확인**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="3f512-135">데이터 연결 선택 대화 상자는 데이터베이스 연결 설정으로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="3f512-136">선택에서 하면 데이터베이스 개체 대화 상자에서 **테이블** 노드를 선택 합니다 **Person** 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
-   <span data-ttu-id="3f512-137">또한 아래에 있는 다음 저장된 프로시저를 선택 합니다 **저장 프로시저 및 함수** 노드: **DeletePerson**합니다 **InsertPerson**, 및 **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="3f512-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span> 
-   <span data-ttu-id="3f512-138">Visual Studio 2012를 사용한 EF 디자이너 시작 저장된 프로시저의 대량 가져오기를 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="3f512-139">합니다 **엔터티 모델에 저장된 프로시저 및 함수를 선택 하는 가져오기** 기본적으로 선택 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="3f512-140">이 예제에서 삽입, 업데이트 및 엔터티 형식을 삭제 하는 프로시저 저장를 있으므로 가져와야 하지 않으려는 하 고이 확인란의 선택을 취소 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span> 

    ![S 프로시저 가져오기](~/ef6/media/importsprocs.jpg)

-   <span data-ttu-id="3f512-142">**마침**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-142">Click **Finish**.</span></span>
    <span data-ttu-id="3f512-143">모델 편집을 위해 디자인 화면을 제공 하는 EF 디자이너 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="3f512-144">저장된 프로시저에 Person 엔터티 매핑</span><span class="sxs-lookup"><span data-stu-id="3f512-144">Map the Person Entity to Stored Procedures</span></span>

-   <span data-ttu-id="3f512-145">마우스 오른쪽 단추로 클릭 합니다 **Person** 엔터티 형식 및 선택 **저장 프로시저 매핑**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
-   <span data-ttu-id="3f512-146">저장된 프로시저 매핑에 표시 합니다 **매핑 정보** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="3f512-147">클릭  **&lt;선택 함수 삽입&gt;** 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="3f512-148">필드가 스토리지 모델의 저장 프로시저를 포함하는 드롭다운 목록이 됩니다. 이 목록의 저장 프로시저를 개념적 모델의 엔터티 형식에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="3f512-149">선택 **InsertPerson** 드롭 다운 목록에서.</span><span class="sxs-lookup"><span data-stu-id="3f512-149">Select **InsertPerson** from the drop-down list.</span></span>
-   <span data-ttu-id="3f512-150">저장 프로시저 매개 변수와 엔터티 속성 간의 기본 매핑이 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="3f512-151">화살표는 매핑 방향을 나타냅니다. 즉, 속성 값이 저장 프로시저 매개 변수에 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
-   <span data-ttu-id="3f512-152">클릭  **&lt;; Result Binding 추가&gt;** 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-152">Click **&lt;Add Result Binding&gt;**.</span></span>
-   <span data-ttu-id="3f512-153">형식 **NewPersonID**에서 반환한 매개 변수의 이름을 합니다 **InsertPerson** 저장 프로시저.</span><span class="sxs-lookup"><span data-stu-id="3f512-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="3f512-154">선행 또는 후행 공백을 입력할 필요가 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-154">Make sure not to type leading or trailing spaces.</span></span>
-   <span data-ttu-id="3f512-155">**Enter** 키를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-155">Press **Enter**.</span></span>
-   <span data-ttu-id="3f512-156">기본적으로 **NewPersonID** 엔터티 키에 매핑되어 **PersonID**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="3f512-157">화살표는 매핑 방향을 나타냅니다. 즉, 결과 열의 값이 속성에 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![매핑 정보](~/ef6/media/mappingdetails.png)

-   <span data-ttu-id="3f512-159">클릭 **&lt;Update Function 선택&gt;** 선택한 **UpdatePerson** 결과 드롭다운 목록에서.</span><span class="sxs-lookup"><span data-stu-id="3f512-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="3f512-160">저장 프로시저 매개 변수와 엔터티 속성 간의 기본 매핑이 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
-   <span data-ttu-id="3f512-161">클릭 **&lt;Delete Function 선택&gt;** 선택한 **DeletePerson** 결과 드롭다운 목록에서.</span><span class="sxs-lookup"><span data-stu-id="3f512-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="3f512-162">저장 프로시저 매개 변수와 엔터티 속성 간의 기본 매핑이 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="3f512-163">삽입, 업데이트 및 삭제 작업을 **Person** 엔터티 형식에 이제 저장된 프로시저에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="3f512-164">업데이트 하거나 저장된 프로시저를 사용 하 여 엔터티를 삭제할 때 확인 하는 동시성을 사용 하도록 설정 하려는 경우 다음 옵션 중 하나를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

-   <span data-ttu-id="3f512-165">사용 하 여는 **출력** 매개 변수 수가 저장된 프로시저 및 검사에서 행에 영향을 합니다 **매개 변수에 영향을 받는 행** 매개 변수 이름 옆의 확인란을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="3f512-166">작업이 호출 될 때 반환 되는 값이 0을 [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
-   <span data-ttu-id="3f512-167">확인 합니다 **Original Value 사용** 동시성 검사에 사용 하려는 속성 옆의 확인란을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="3f512-168">원래 데이터베이스에서 읽은 속성의 값을 업데이트 하려고 하는 경우 데이터 쓰기 데이터베이스에 백업 하는 경우 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="3f512-169">값을 데이터베이스에서 값을 일치 하지 않는 경우는 **OptimisticConcurrencyException** throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="3f512-170">모델 사용</span><span class="sxs-lookup"><span data-stu-id="3f512-170">Use the Model</span></span>

<span data-ttu-id="3f512-171">열기는 **Program.cs** 파일을 **Main** 메서드 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="3f512-172">다음 코드를 Main 함수에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="3f512-173">코드를 만듭니다 **Person** 개체 다음 개체를 업데이트 하 고 마지막으로 개체를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>         

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

-   <span data-ttu-id="3f512-174">애플리케이션을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-174">Compile and run the application.</span></span> <span data-ttu-id="3f512-175">다음 출력을 생성 하는 프로그램 \*</span><span class="sxs-lookup"><span data-stu-id="3f512-175">The program produces the following output \*</span></span>
    >[!NOTE]
> <span data-ttu-id="3f512-176">PersonID 이므로 서버에서 자동으로 생성 하면 표시 될 가능성이 다른 번호 \*</span><span class="sxs-lookup"><span data-stu-id="3f512-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="3f512-177">Visual Studio Ultimate 버전을 사용 하는 경우에 실행 되는 SQL 문을 보려면 디버거를 사용 하 여 Intellitrace를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f512-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![Intellitrace](~/ef6/media/intellitrace.png)
