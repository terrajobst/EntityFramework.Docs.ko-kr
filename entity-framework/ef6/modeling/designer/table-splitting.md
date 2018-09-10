---
title: 디자이너 테이블 분할 EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f07aeb0aa679f6fa8131c667ac808f17c3f03f20
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250987"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="8e837-102">디자이너의 테이블 분</span><span class="sxs-lookup"><span data-stu-id="8e837-102">Designer Table Splitting</span></span>
<span data-ttu-id="8e837-103">이 연습에서는 Entity Framework Designer (EF 디자이너)를 사용 하 여 모델을 수정 하 여 여러 엔터티 형식을 단일 테이블에 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="8e837-104">테이블 분할을 사용 하려는 이유 중 하나는 지연에 개체를 로드 하려면 로드를 사용 하는 경우 일부 속성을 로드 하는 지연 되 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span> <span data-ttu-id="8e837-105">별도 엔터티로 매우 많은 양의 데이터를 포함 하는 수만 필요할 때 로드할 속성을 구분할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-105">You can separate the properties that might contain very large amount of data into a seperate entity and only load it when required.</span></span>

<span data-ttu-id="8e837-106">다음 이미지에서는 EF 디자이너를 사용 하 여 작업할 때 사용 되는 기본 windows를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF 디자이너](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="8e837-108">전제 조건</span><span class="sxs-lookup"><span data-stu-id="8e837-108">Prerequisites</span></span>

<span data-ttu-id="8e837-109">이 연습을 완료하려면 다음 사항이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="8e837-110">Visual Studio의 최신 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="8e837-111">합니다 [School 샘플 데이터베이스](~/ef6/resources/school-database.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="8e837-112">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="8e837-112">Set up the Project</span></span>

<span data-ttu-id="8e837-113">이 연습에서는 Visual Studio 2012 사용 중입니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="8e837-114">Visual Studio 2012를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="8e837-115">**파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="8e837-116">왼쪽된 창에서 Visual C 클릭\#, 콘솔 응용 프로그램 템플릿을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="8e837-117">입력 **TableSplittingSample** 고 프로젝트의 이름으로 **확인**합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="8e837-118">School 데이터베이스를 기반으로 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="8e837-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="8e837-119">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 **추가**를 클릭 하 고 **새 항목**합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="8e837-120">선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서.</span><span class="sxs-lookup"><span data-stu-id="8e837-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="8e837-121">입력 **TableSplittingModel.edmx** 파일 이름 및 클릭 **추가**합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="8e837-122">Model 콘텐츠 선택 대화 상자에서 선택 **데이터베이스에서 생성**를 클릭 하 고 **다음입니다.**</span><span class="sxs-lookup"><span data-stu-id="8e837-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="8e837-123">새 연결을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-123">Click New Connection.</span></span> <span data-ttu-id="8e837-124">연결 속성 대화 상자에서 서버 이름을 입력 합니다 (예를 들어 **(localdb)\\mssqllocaldb**)을 선택 인증 방법, 형식 **학교** 데이터베이스 이름에 대 한 다음 클릭 **확인**합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="8e837-125">데이터 연결 선택 대화 상자는 데이터베이스 연결 설정으로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="8e837-126">데이터베이스 개체 선택 대화 상자에서 펼침를 **테이블** 노드와 확인 합니다 **Person** 테이블.</span><span class="sxs-lookup"><span data-stu-id="8e837-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="8e837-127">이 지정 된 테이블을 추가 합니다 **학교** 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="8e837-128">**마침**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-128">Click **Finish**.</span></span>

<span data-ttu-id="8e837-129">모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="8e837-130">선택한 모든 개체를 **데이터베이스 개체 선택** 대화 상자는 모델에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="8e837-131">단일 테이블에 두 개의 엔터티 매핑</span><span class="sxs-lookup"><span data-stu-id="8e837-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="8e837-132">분할이 섹션의 **Person** 두 엔터티를 엔터티 단일 테이블에 매핑하면 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="8e837-133">합니다 **Person** 엔터티에 많은 양의 데이터를 포함할 수 있는 모든 속성을 포함 되지 않는; 예로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="8e837-134">디자인 화면의 빈 영역을 마우스 오른쪽 **새로 추가**, 클릭 **엔터티**합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="8e837-135">합니다 **새 엔터티** 대화 상자가 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="8e837-136">형식 **HireInfo** 에 대 한 합니다 **엔터티 이름** 및 **PersonID** 에 대 한 합니다 **키 속성** 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="8e837-137">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-137">Click **OK**.</span></span>
-   <span data-ttu-id="8e837-138">새 엔터티 형식이 만들어져 디자인 화면에 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="8e837-139">선택는 **HireDate** 의 속성을 **사용자** 엔터티 형식 및 키를 누릅니다 **Ctrl + X** 키.</span><span class="sxs-lookup"><span data-stu-id="8e837-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="8e837-140">선택 된 **HireInfo** 엔터티와 키를 눌러 **Ctrl + V** 키입니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="8e837-141">간에 관계를 만들려면 **Person** 하 고 **HireInfo**합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="8e837-142">이렇게 하려면 디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭, 가리킨 **새로 추가**, 클릭 **연결**합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="8e837-143">합니다 **연결 추가** 대화 상자가 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="8e837-144">합니다 **PersonHireInfo** 기본적으로 이름이 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="8e837-145">복합성을 지정 **1(One)** 관계의 양쪽 끝에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="8e837-146">키를 눌러 **확인**합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-146">Press **OK**.</span></span>

<span data-ttu-id="8e837-147">다음 단계에 필요 합니다 **매핑 정보** 창입니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="8e837-148">이 창에 표시 되지 않으면 디자인 화면을 마우스 오른쪽 단추로 **매핑 정보**합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="8e837-149">선택 합니다 **HireInfo** 엔터티 형식 및 클릭 **&lt;테이블 또는 뷰 추가&gt;** 에 **매핑 정보** 창.</span><span class="sxs-lookup"><span data-stu-id="8e837-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="8e837-150">선택 **Person** 에서 **&lt;테이블이 나 뷰 추가&gt;** 필드 드롭 다운 목록.</span><span class="sxs-lookup"><span data-stu-id="8e837-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="8e837-151">목록 테이블이 또는 선택한 엔터티에 매핑할 수 있는 뷰.</span><span class="sxs-lookup"><span data-stu-id="8e837-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="8e837-152">기본적으로 적절 한 속성을 매핑해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-152">The appropriate properties should be mapped by default.</span></span>

    ![매핑](~/ef6/media/mapping.png)

-   <span data-ttu-id="8e837-154">선택 된 **PersonHireInfo** 디자인 화면에 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="8e837-155">디자인 화면에서 연결을 마우스 오른쪽 단추로 클릭 **속성**합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="8e837-156">에 **속성** 창에서를 **참조 제약 조건** 속성 줄임표 단추를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="8e837-157">선택 **Person** 에서 합니다 **주체** 드롭 다운 목록.</span><span class="sxs-lookup"><span data-stu-id="8e837-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="8e837-158">키를 눌러 **확인**합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="8e837-159">모델 사용</span><span class="sxs-lookup"><span data-stu-id="8e837-159">Use the Model</span></span>

-   <span data-ttu-id="8e837-160">Main 메서드에 다음 코드를 붙여 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-160">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   <span data-ttu-id="8e837-161">응용 프로그램을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-161">Compile and run the application.</span></span>

<span data-ttu-id="8e837-162">다음 T-SQL 문을 대해 실행 된 합니다 **학교** 이 응용 프로그램을 실행 한 결과로 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="8e837-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="8e837-163">다음 **삽입** 컨텍스트를 실행 한 결과로 실행 되었습니다. savechanges () 및 결합 하 여 데이터를 **Person** 하 고 **HireInfo** 엔터티</span><span class="sxs-lookup"><span data-stu-id="8e837-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Insert](~/ef6/media/insert.png)

-   <span data-ttu-id="8e837-165">다음 **선택** 컨텍스트를 실행 한 결과로 실행 되었습니다. 에 매핑된 열만 선택 하 고 People.FirstOrDefault() **사람**</span><span class="sxs-lookup"><span data-stu-id="8e837-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![1 선택](~/ef6/media/select1.png)

-   <span data-ttu-id="8e837-167">다음 **선택** 탐색 속성 existingPerson.Instructor 액세스의 결과로 실행 되 고에 매핑된 열만 선택 **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="8e837-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![2를 선택 합니다.](~/ef6/media/select2.png)
