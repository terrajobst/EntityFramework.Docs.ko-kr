---
title: 쿼리-EF 디자이너-EF6를 정의합니다.
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489481"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="fbdf5-102">정의 쿼리-EF 디자이너</span><span class="sxs-lookup"><span data-stu-id="fbdf5-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="fbdf5-103">이 연습에서는 정의 추가 하는 방법을 보여 줍니다. 쿼리 및 해당 엔터티를 EF 디자이너를 사용 하 여 모델에 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="fbdf5-104">정의 쿼리는 일반적으로 데이터베이스 뷰를 제공 하는 유사한 기능을 제공 하는 데 사용 됩니다 있지만 뷰는 데이터베이스가 아닌 모델에서 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="fbdf5-105">정의 쿼리를 사용 하면 지정 된 SQL 문을 실행 하는 **DefiningQuery** .edmx 파일의 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="fbdf5-106">자세한 내용은 **DefiningQuery** 에 [SSDL 사양](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="fbdf5-107">정의 쿼리를 사용 하는 경우 모델에 엔터티 형식을 정의할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="fbdf5-108">엔터티 형식 정의 쿼리에 의해 노출 된 데이터를 노출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="fbdf5-109">참고가 엔터티 형식을 통해 표시 되는 데이터는 읽기 전용입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="fbdf5-110">매개 변수가 있는 쿼리를 정의 쿼리로 실행할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="fbdf5-111">그러나 데이터는 저장 프로시저에 데이터를 표시하는 엔터티 형식의 삽입, 업데이트 및 삭제 함수를 매핑하여 업데이트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="fbdf5-112">자세한 내용은 [삽입, 업데이트 및 삭제 저장 프로시저를 사용 하 여](~/ef6/modeling/designer/stored-procedures/cud.md)입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="fbdf5-113">이 항목에서는 다음 작업을 수행 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="fbdf5-114">정의 쿼리 추가</span><span class="sxs-lookup"><span data-stu-id="fbdf5-114">Add a Defining Query</span></span>
-   <span data-ttu-id="fbdf5-115">모델에 엔터티 형식 추가</span><span class="sxs-lookup"><span data-stu-id="fbdf5-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="fbdf5-116">맵 엔터티 형식으로 정의 쿼리</span><span class="sxs-lookup"><span data-stu-id="fbdf5-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fbdf5-117">전제 조건</span><span class="sxs-lookup"><span data-stu-id="fbdf5-117">Prerequisites</span></span>

<span data-ttu-id="fbdf5-118">이 연습을 완료하려면 다음 사항이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="fbdf5-119">Visual Studio의 최신 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="fbdf5-120">합니다 [School 샘플 데이터베이스](~/ef6/resources/school-database.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="fbdf5-121">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="fbdf5-121">Set up the Project</span></span>

<span data-ttu-id="fbdf5-122">이 연습에서는 Visual Studio 2012 이상 사용 중입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="fbdf5-123">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="fbdf5-124">**파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="fbdf5-125">왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔 응용 프로그램** 템플릿.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="fbdf5-126">입력 **DefiningQuerySample** 고 프로젝트의 이름으로 **확인**합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="fbdf5-127">School 데이터베이스를 기반으로 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="fbdf5-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="fbdf5-128">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 **추가**를 클릭 하 고 **새 항목**합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="fbdf5-129">선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="fbdf5-130">입력 **DefiningQueryModel.edmx** 파일 이름 및 클릭 **추가**합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="fbdf5-131">Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음**합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="fbdf5-132">새 연결을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-132">Click New Connection.</span></span> <span data-ttu-id="fbdf5-133">연결 속성 대화 상자에서 서버 이름을 입력 합니다 (예를 들어 **(localdb)\\mssqllocaldb**)을 선택 인증 방법, 형식 **학교** 데이터베이스 이름에 대 한 다음 클릭 **확인**합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="fbdf5-134">데이터 연결 선택 대화 상자는 데이터베이스 연결 설정으로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="fbdf5-135">데이터베이스 개체 선택 대화 상자에서 확인 합니다 **테이블** 노드.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="fbdf5-136">이 테이블을 모두 추가 합니다 **학교** 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="fbdf5-137">**마침**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-137">Click **Finish**.</span></span>
-   <span data-ttu-id="fbdf5-138">솔루션 탐색기에서 마우스 오른쪽 단추로 클릭 합니다 **DefiningQueryModel.edmx** 파일을 선택 **연결 프로그램...** .</span><span class="sxs-lookup"><span data-stu-id="fbdf5-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="fbdf5-139">선택 **XML (텍스트) 편집기**합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-139">Select **XML (Text) Editor**.</span></span>

    ![XML 편집기](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="fbdf5-141">클릭 **예** 다음 메시지와 함께 메시지가 표시 되 면:</span><span class="sxs-lookup"><span data-stu-id="fbdf5-141">Click **Yes** if prompted with the following message:</span></span>

    ![경고 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="fbdf5-143">정의 쿼리 추가</span><span class="sxs-lookup"><span data-stu-id="fbdf5-143">Add a Defining Query</span></span>

<span data-ttu-id="fbdf5-144">정의 추가 하려면 XML 편집기를 사용 하 여이 단계에서는 쿼리 및.edmx 파일의 SSDL 섹션에는 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="fbdf5-145">추가 된 **EntitySet** (13 년에서 5 줄).edmx 파일의 SSDL 섹션에는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="fbdf5-146">다음을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-146">Specify the following:</span></span>
    -   <span data-ttu-id="fbdf5-147">만 **이름** 및 **EntityType** 의 특성을 **EntitySet** 요소를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="fbdf5-148">엔터티 형식의 정규화 된 이름에 사용 되는 **EntityType** 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="fbdf5-149">에 지정 된 SQL 문을 실행 합니다 **DefiningQuery** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   <span data-ttu-id="fbdf5-150">추가 된 **EntityType** 요소를.edmx 파일의 SSDL 섹션에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="fbdf5-151">표시 된 것 처럼 아래 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-151">file as shown below.</span></span> <span data-ttu-id="fbdf5-152">다음 사항에 유의하십시오.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-152">Note the following:</span></span>
    -   <span data-ttu-id="fbdf5-153">값을 **이름** 특성의 값에 해당는 **EntityType** 특성를 **EntitySet** 요소 위의 있지만의 정규화 된 이름을 엔터티 형식에 사용 되는 **EntityType** 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="fbdf5-154">속성 이름은 SQL 문에서 반환 되는 열 이름에 해당 합니다 **DefiningQuery** 요소 (위의 설명 참조).</span><span class="sxs-lookup"><span data-stu-id="fbdf5-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="fbdf5-155">이 예제에서 엔터티 키는 고유 키 값을 보장하는 세 개의 속성으로 구성되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> <span data-ttu-id="fbdf5-156">나중에 실행 하는 경우는 **모델 업데이트 마법사** 대화 상자에서 정의 쿼리를 포함 하 여 저장소 모델에 대 한 변경 내용을 덮어쓰게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="fbdf5-157">모델에 엔터티 형식 추가</span><span class="sxs-lookup"><span data-stu-id="fbdf5-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="fbdf5-158">이 단계에서는 EF 디자이너를 사용 하 여 개념적 모델에 엔터티 형식을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span>  <span data-ttu-id="fbdf5-159">다음 사항에 유의하십시오.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-159">Note the following:</span></span>

-   <span data-ttu-id="fbdf5-160">**이름** 엔터티의 값에 해당 하는 **EntityType** 특성을 **EntitySet** 위의 요소.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="fbdf5-161">속성 이름은 SQL 문에서 반환 되는 열 이름에 해당 합니다 **DefiningQuery** 위의 요소.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="fbdf5-162">이 예제에서 엔터티 키는 고유 키 값을 보장하는 세 개의 속성으로 구성되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="fbdf5-163">EF 디자이너에서 모델을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="fbdf5-164">DefiningQueryModel.edmx를 두 번 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="fbdf5-165">예를 들어 **예** 다음 메시지:</span><span class="sxs-lookup"><span data-stu-id="fbdf5-165">Say **Yes** to the following message:</span></span>

    ![경고 2](~/ef6/media/warning2.png)

 

<span data-ttu-id="fbdf5-167">모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="fbdf5-168">화면 디자이너를 마우스 오른쪽 단추로 클릭 **새로 추가**-&gt;**엔터티 중...** .</span><span class="sxs-lookup"><span data-stu-id="fbdf5-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="fbdf5-169">지정할 **GradeReport** 엔터티 이름에 대 한 및 **CourseID** 에 대 한 합니다 **키 속성**합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="fbdf5-170">마우스 오른쪽 단추로 클릭 합니다 **GradeReport** 엔터티 및 선택 **새로 추가** - &gt; **스칼라 속성**합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="fbdf5-171">속성의 기본 이름을 변경할 **FirstName**합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="fbdf5-172">다른 스칼라 속성을 추가 하 고 지정할 **LastName** 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="fbdf5-173">다른 스칼라 속성을 추가 하 고 지정할 **등급** 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="fbdf5-174">에 **속성** 창에서 변경 합니다 **등급**의 **형식** 속성을 **10 진수**.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="fbdf5-175">선택 된 **FirstName** 하 고 **LastName** 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="fbdf5-176">에 **속성** 창에서 변경 합니다 **EntityKey** 속성 값을 **True**합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="fbdf5-177">결과적으로, 다음 요소를 추가한 합니다 **CSDL** .edmx 파일의 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="fbdf5-178">맵 엔터티 형식으로 정의 쿼리</span><span class="sxs-lookup"><span data-stu-id="fbdf5-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="fbdf5-179">이 단계에서는 개념 매핑할 매핑 정보 창 및 저장소 엔터티 형식을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="fbdf5-180">마우스 오른쪽 단추로 클릭 합니다 **GradeReport** 디자인 화면에서 엔터티 **테이블 매핑**합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="fbdf5-181">합니다 **매핑 정보** 창이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="fbdf5-182">선택 **GradeReport** 에서 합니다 **&lt;테이블이 나 뷰 추가&gt;** 드롭다운 목록 (아래에 있는 **테이블**s).</span><span class="sxs-lookup"><span data-stu-id="fbdf5-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="fbdf5-183">기본 개념 간의 매핑 및 저장소 **GradeReport** 엔터티 형식을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="fbdf5-184">![Details3 매핑](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="fbdf5-184">![Mapping Details3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="fbdf5-185">결과적으로 **EntitySetMapping** 요소가.edmx 파일의 매핑 섹션에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   <span data-ttu-id="fbdf5-186">응용 프로그램을 컴파일합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="fbdf5-187">코드에서 정의 쿼리를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="fbdf5-188">사용 하 여 이제 정의 쿼리를 실행할 수 있습니다 합니다 **GradeReport** 엔터티 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="fbdf5-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
